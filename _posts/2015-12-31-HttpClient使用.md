---
layout: post
title: HttpClient使用
---

### http几个TimeOut的区别

#### 连接超时(ConnectTimeOut): 
client去连接server，这是一个tcp过程，存在3次握手。client的```SYN```已经给到server，但server的```SYN+ACK```没有给到client。

建立连接的3次握手如下图：

![tcp建立连接 3次握手](http://changer119.qiniudn.com/QQ20151231-0@2x.png)

**如果client去访问一个不存在的url，是否会抛出ConnectTimeOut？**

我在MacPro的机器上尝试了一下，client会立马报Connectioin Refused错误。

#### 请求超时(SocketTimeOut): 

client已经与server成功建立连接，并且server在做逻辑处理，但这个处理时间过程，还没来得及返回client结果，client端就报SocketTimeOut。

这个可以在server端通过sleep的方式模拟出来。


### 重试机制

HttpClient提供了应用层面的重试机制。参照下面的代码。

``` java
@Test
public void testRetry() throws IOException {
    String orgTxNo = "EC7FF7817B894AD19C7C6A3CE060A7AF";
    String url = host + "/trusteepay/txstatusquery?orgTxNo=" + orgTxNo;
    
    // 实现HttpRequestRetryHandler接口
    // 只有httpClient.execute(method)抛出异常时，才会进入myRetryHandler。
    HttpRequestRetryHandler myRetryHandler = new HttpRequestRetryHandler() {
        @Override
        public boolean retryRequest(IOException e, int i, HttpContext httpContext) {
            if (i >= 5) {
                return false;
            }
            /*if (e instanceof InterruptedIOException) {
                return false;
            }*/
            if (e instanceof UnknownHostException) {
                return false;
            }
            if (e instanceof ConnectTimeoutException) {
                return false;
            }
            if (e instanceof SSLException) {
                return false;
            }
            HttpClientContext clientContext = HttpClientContext.adapt(httpContext);
            HttpRequest request = clientContext.getRequest();
            boolean idempotent = !(request instanceof HttpEntityEnclosingRequest);
            if (idempotent) {
                // Retry if the request is considered idempotent
                return true;
            }
            return false;
        }
    };
    HttpClient httpClient = HttpClients.custom()
            .setRetryHandler(myRetryHandler)
            .build();
    HttpGet httpGet = new HttpGet(url);
    RequestConfig config = RequestConfig.custom()
            .setConnectTimeout(2000)    //连接超时 2s
            .setSocketTimeout(5000)    //请求超时 10s
            .build();
    httpGet.setConfig(config);
    try{
        HttpResponse httpResponse = httpClient.execute(httpGet);
        System.out.println("statusLine: " + httpResponse.getStatusLine().toString());
        HttpEntity httpEntity = httpResponse.getEntity();
        System.out.println("entity: " + EntityUtils.toString(httpEntity));
    }
    catch (Exception e){
        System.out.println("产生异常========");
        e.printStackTrace();
        if (e instanceof IOException)
            // 记得抛出异常，或者不进行异常捕获
            throw new IOException(e);
    }
    finally { // 这里的finally会在重试5次之后执行，5次重试用的同一个httpGet连接
        System.out.println(MessageFormat.format("释放连接 hashCode={0}", httpGet.hashCode()));
        httpGet.releaseConnection();
    }
}
```


