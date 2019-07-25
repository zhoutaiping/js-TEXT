#TCP协议提供可靠的连接服务,采用三次握手建立一个连接 (客户端与服务端链接响应过程)

```
1: A-->B  
-建立连接时,客户端发送syn包(syn=j)到服务器,并进入SYN_SEND状态,等待服务器确认；
-SYN：同步序列编号(Synchronize Sequence Numbers)
2: B-->A
-服务器收到syn包,必须确认客户的SYN（ack=j+1）,同时自己也发送一个SYN包（syn=k）,即SYN+ACK包,此时服务器进入SYN_RECV状态；
3: A-->B
-客户端收到服务器的SYN＋ACK包,向服务器发送确认包ACK(ack=k+1),此包发送完毕,客户端和服务器进入ESTABLISHED状态,完成三次握手

```
#完成三次握手,客户端与服务器开始传送数据

#A与B建立TCP连接时：首先A向B发SYN（同步请求），然后B回复SYN搜索+ACK（同步请求应答），最后A回复ACK确认，这样TCP的一次连接（三次握手）的过程就建立了！
