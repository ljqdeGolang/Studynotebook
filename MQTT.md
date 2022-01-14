## MQTT

MQTT是一个客户端服务端架构的发布/订阅模式的消息传输协议。它的设计思想是轻巧、开放、简单、规范，易于实现。这些特点使得它对很多场景来说都是很好的选择，特别是对于受限的环境如机器与机器的通信（M2M）以及物联网环境（IoT）。

### 术语：

客户端client

服务端Server

订阅Subscription

主题名Topic Name

主体过滤器 Topic Filter

会话 Session

控制报文 MQTT Control Packet

### 控制报文格式

结构：

| Fixed header    | 固定报头，所有控制报文都包含 |
| --------------- | ---------------------------- |
| Variable header | 可变报头，部分控制报文包含   |
| Payload         | 有效载荷，部分控制报文包含   |

#### CONNECT—连接到服务端

客户端到服务端的网络连接建立后，客户端发送给服务端的第一个报文**必须**是CONNECT报文 。

固定报头 Fixed header

可变报头Variable header

有效载荷Payload

响应Response

#### CONNACK—确认连接请求

服务端发送CONNACK报文响应从客户端收到的CONNECT报文。服务端发送给客户端的第一个报文**必须**是CONNACK。

#### PUBLISH—发布请求

PUBLISH控制报文是指从客户端向服务端或者服务端向客户端传输一个应用消息。

