# httpAgent
本库旨在简化 Aardio HP-Socket 库的繁琐调用及线程回调同步问题，通过简单封装以方便的使用HP Socket的HTTP请求。

本库通过创建HTTP及HTTPS两个对象，完成HTTP/HTTPS/WS/WSS的支持。

需要注意的是，HP-Socket的SSL对象与SSL证书模式具有绑定模式，所以本库仅适用于一般的HTTPS请求的连接复用。
需要使用专用证书的不同请求，请创建多个对象。

httpAgent默认同一个网站的请求限制为5个，请求完成后，同一个网站、同一个端口的连接会自动分配给下一个请求。
httpAgent的任务管理通过HP-Socket的回调触发，未单独设定定时器，onMessageComplet及onClose均会触发任务管理，已分配下一个任务对象。

httpAgent的最终回调会返回 statue, task, enOperation, errCode, existWebSocket. 五个参数。
statue表示任务状态，正常是1，断开为-1，Websocket为4；
task 针对HTTP及Websocket会返回不同对象。

    HTTP直接返回任务对象，其中task.responese是服务器返回内容对象；
        response.headers 请参考 hpsocket.helper.headers 的用法；
        
    Websocket连接失败时返回HTTP任务对象，成功后返回Websocket对象；
        Websocket对象已提供 send/close 两个方法，以便发送数据和关闭连接；
        Websocket对象同时提供 onWsMessage/onClose 两个回调函数以便接受数据和获取关闭消息；




由于Aardio的HP-Socket库自身的问题：
1.使用本库前请更新 hpsocket.socket.httpAgent 库，以便 Websocket 完成协议切换。
2.请更新 hpsocket.ssl 库，以修正 SSL 函数未更新带来的问题。

感谢 伤神小怪兽 提供的 HP-Socket 源码，项目地址：https://gitee.com/ldcsaa/HP-Socket
