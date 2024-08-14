# HTTP
* 学习网址：https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Overview
------------
## HTTP概述
* HTTP 是一种用作获取诸如 HTML 文档这类资源的协议。
* 客户端发出的消息被称作请求（request），由服务端发出的应答消息被称作响应（response）
* HTTP 是一种应用层的协议
* 浏览器总是首先发起请求的那个实体，永远不会是服务端
--------
## 代理（Proxy）作用:
* 缓存（可以是公开的也可以是私有的，如浏览器的缓存）
* 过滤（如反病毒扫描、家长控制...）
* 负载均衡（让多个服务器服务不同的请求）
* 认证（控制对不同资源的访问）
* 日志（使得代理可以存储历史信息）
-------
## 基本性质
* 简约、可扩展（HTTP标头）、无状态，但并非无对话、连接（传输层，TCP）
--------
## HTTP能控制什么
* 缓存：服务端能指示代理和客户端缓存哪些内容以及缓存多长时间，客户端能够指示中间的缓存代理来忽略已存储的文档
* 开放同源限制：只有来自于相同来源（same origin）的网页才能够获取一个网页的全部信息
* 认证：一些页面可能会被保护起来，仅让特定的用户进行访问
* 代理服务器和隧道：服务器或客户端常常是处于内网的，对其他计算机隐藏真实 IP 地址，因此 HTTP 请求就要通过代理服务器越过这个网络屏障
* 会话：使用 HTTP Cookie 可以利用服务端的状态将不同请求联系在一起
------
## HTTP报文
### 请求
* 组成元素
1. HTTP方法：通常是由一个动词，像 GET、POST 等，或者一个名词，像 OPTIONS、HEAD 等，来定义客户端执行的动作
2. 要获取的那个资源的路径
3. HTTP 协议版本号
4. 为服务端表达其他信息的可选标头
5. 请求体（body）
------
### 响应
* 响应报文包含元素
1. HTTP 协议版本号。
2. 状态码
3. 状态信息
4. HTTP 标头
5. 可选项
--------
## 基于HTTP的API
* Fetch API 是基于 HTTP 的最常用 API，其可用于在 JavaScript 中发起 HTTP 请求
* 另一种 API，server-sent 事件，是一种单向服务，允许服务端借助作为 HTTP 传输机制向客户端发送事件。使用 EventSource 接口，客户端可打开连接并创建事件处理器。客户端浏览器自动将 HTTP 流里到达的消息转换为适当的 Event 对象。继而将已知类型的事件，传递给先前注册过的事件处理器，其他未指明类型的事件则传递给 onmessage 事件处理器。
--------
## 万维网（World Wide Web）的组成
1. 一个用来表示超文本文档的文本格式，超文本标记语言（HTML）
2. 一个用来交换超文本文档的简单协议，超文本传输协议（HTTP）
3. 一个显示（以及编辑）超文本文档的客户端，即网络浏览器
4. 一个服务器用于提供可访问的文档
-------
## HTTP扩展
* Server-sent events，服务器可以偶尔推送消息到浏览器
* WebSocket，一个新协议，可以通过升级现有 HTTP 协议来建立
* 跨源资源共享（CORS）
* 内容安全策略（CSP）
* DNT（Do Not Track）来控制隐私
* 客户端提示（client hint）
----------
## HTTP消息
* HTTP 消息是服务器和客户端之间交换数据的方式，由采用 ASCII 编码的多行文本构成
* HTTP 请求是由客户端发出的消息，用来使服务器执行动作
------------
### 起始行（start-line）包含三个元素：
1. 一个 HTTP 方法
2. 请求目标（request target）
3. HTTP 版本（HTTP version）
-----------------
### 标头（Header）
1. 通用标头（General header）
2. 请求标头（Request header）
3. 表示标头（Representation header）
--------------
### 主体（Body）
1. 单一资源（Single-resource）主体，由一个单文件组成，该类型的主体由两个标头定义：Content-Type 和 Content-Length
2. 资源（Multiple-resource）主体，由多部分主体组成
------------
## HTTP响应
### 状态行
* HTTP 响应的起始行被称作状态行（status line），包含：
1. 协议版本
2. 状态码（status code）
3. 状态文本（status text）
例如：HTTP/1.1 404 Not Found
--------
### 标头（Header）
1. 通用标头（General header）
2. 响应标头（Response header）
3. 表示标头（Representation header）
----------
### 主体（Body）
1. 单资源（Single-resource）主体，由已知长度的单个文件组成
2. 单资源（Single-resource）主体，由未知长度的单个文件组成
3. 多资源（Multiple-resource）主体
-------------











