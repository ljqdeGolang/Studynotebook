### websocket学习笔记

Web socket的一大特点是：服务器可以主动向客户端推送信息，客户端也可以主动向服务器发送信息，是真正的双向平等对话，属于服务器推送技术的一种。

<img src="D:\git-library\Studynotebook\figure\bg2017051503.jpg" alt="bg2017051503"  />

在我们的项目中，我选择运用适合go语言的gorilla/websocket框架：

- 创建Upgrader实例用于升级请求
- 用ReadMessage()和WriteMessage()来读写消息

