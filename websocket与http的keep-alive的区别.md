# websocket与http的keep-alive的区别

### HTTP协议的发展历程

​	1.HTTP0.9\color{red}{HTTP 0.9}HTTP0.9是第一个版本的HTTP协议，它具有HTTP协议典型的无状态的特点，即每个请求独立进行处理，响应结束时就释放这个连接。

​	2.HTTP1.0\color{red}{HTTP 1.0}HTTP1.0开始支持长连接（但默认还是使用短连接）。如果使用长连接，需要添加请求头`Connection:keep-alive`。

​	3.HTTP1.1\color{red}{HTTP 1.1}HTTP1.1起，浏览器和服务器默认开启Keep-Alive。客户端和服务器都能选择随时关闭连接，则请求头中为`Connection:close`。

### 1、短连接

​	在HTTP 0.9和HTTP 1.0中默认使用短连接。也就是说，**客户端和服务器每进行一次HTTP操作，就建立一次连接，任务结束就中断连接。**

### 2、长连接

​	而使用长连接后，当一个网页打开完成后，**客户端和服务器之间用于传输HTTP数据的TCP连接不会关闭，客户端再次访问这个服务器时，会继续使用这一条已经建立的连接。**

​	Keep-Alive不会永久保持连接，它有一个保持时间，可以在不同的服务器软件（如Apache）中设定这个时间。实现长连接需要客户端和服务端都支持长连接。

### 3、websocket的长连接

​	当建立起WebSocket的长连接，双方都可以互相发送信息，服务端可以主动发起信息。

**长连接与持久连接的区别**

​		1.通常所说的HTTP长连接和短连接不够严谨，毕竟HTTP是应用层的协议，真正来说应该是TCP的长连接和短接，因为TCP才是传输层的协议。

​		2.建立TCP的长连接后，在一次 TCP 连接中可以完成多个 HTTP 请求，但是对每个请求仍然要单独发请求头。而WebSocket的长连接，只需要经过一次HTTP请求，就可以做到源源不断的信息传送了。所以为了区分两者，可以把TCP的连接称为持久连接，WebSocket的长连接才是真正的长连接。

​		3.HTTP和Websocket同属于应用层协议。HTTP长连接的本质还是HTTP协议，工作模式依旧是一问一答。即：客户端发起一次请求，服务器回应最多一次响应。这个本质并没有得到改变，改变的只是在同一个TCP连接上可以进行多次请求和多次响应。Websocket不一样，客户端可以只请求一次服务器，然后服务器返回多次响应。即：当连接建立之后，服务器可以主动给客户端发送信息，这点是HTTP协议做不到的。



参考链接

[HTTP长连接和Websocket](https://cloud.tencent.com/developer/article/1915361)

[HTTP长连接和Webcoket](https://cloud.tencent.com/developer/article/1915361)



