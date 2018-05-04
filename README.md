# JsBridgePlus
Merge Android and iOS js交互库(JsBridge和WebViewJavascriptBridge)

> 应项目需求统一Android和iOS WebView交互框架

## Javascript
- 统一加载js文件

```Html
<script type="text/javascript" src="mobile_jsbridge.min.js"></script>
```

- js代码写法

```javascript
//send   发送数据
window.WebViewJavascriptBridge.send(data, function(responseData) {
                document.getElementById("show").innerHTML = "repsonseData from java, data " + responseData
            });

//register
bridge.registerHandler('注册Js方法Native调用', function(data, responseCallback) {
        log('ObjC called testJavascriptHandler with', data)
        var responseData = { 'Javascript Says':'Right back atcha!' }
        log('JS responding with', responseData)
        responseCallback(responseData)
});

//callHandler 
window.WebViewJavascriptBridge.callHandler('调用Native注册的方法', 
                                           {'param': '中文测试'}, 
                                           function(responseData) {
});
```



## html初始化
```javascript
setupWebViewJavascriptBridge(function(bridge) {
        //这里注册js方法给Native层调用;
        bridge.registerHandler('functionInJs', function(data, responseCallback) {
            console.log('ObjC called testJavascriptHandler with', data)
            var responseData = { 'Javascript Says':'Right back atcha!' }
            log('JS responding with', responseData)
            responseCallback(responseData)
        });
    });
```

## Android

- 地址:[JsBridge](https://github.com/xieyangxuejun/JsBridge)


- 其中主要的方法是
    - send

    ```Java
    //send ==> 发送消息 {"data":"hello"}
    webview.send("hello");
    ```

    - registerHandler
    - callHandler

- 5/4 新增注解方式注册方法和默认处理方式
```java
webView.loadUrl("file:///android_asset/demo.html");
//使用管理类注册方法
BridgeHandlerManager.getInstance().registerHandler(this, webView);
```

并在this==>如Activity中写注解方法
```Java
//处理模式默认为handlerMode=REGISTER;
//注册submitFromWeb给js调用
@JavascriptInterface
public void submitFromWeb() {
    Log.d("test", "===================================submitFromWeb");
}
```

js调用send方法Java注解处理方式

```
//js调用send方法 Native处理方法
@JavascriptInterface(handlerMode = HandlerMode.SEND)
public void hello() {
    Log.d("test", "===================================hello");
}
```



## iOS

- 地址:WebViewJavascriptBridge](https://github.com/xieyangxuejun/WebViewJavascriptBridge)


- 其中主要的方法是
  - send
  - registerHandler
  - callHandler


- 5/4 js代码中新增send方法.可以向Android那样send发送特性消息;


```objective-c
//send ==> 发送消息 {"data":"hello"}
[_bridge send:@"hello"];

//register ==> 注册Native方法名给JS调用
[_bridge registerHandler:@"testObjcCallback" handler:^(id data, WVJBResponseCallback responseCallback) {
        NSLog(@"testObjcCallback called: %@", data);
        responseCallback(@"Response from testObjcCallback");
}];

//callHandler ==> js注册testJavascriptHandler方法Native调用并传数据
[_bridge callHandler:@"testJavascriptHandler" data:@{ @"foo":@"before ready" }];
```



## 感谢

- [JsBridge](https://github.com/lzyzsd/JsBridge)
- [WebViewJavascriptBridge](https://github.com/marcuswestin/WebViewJavascriptBridge)










