# HTTP相关知识

* HTTP 消息结构[REF](https://www.runoob.com/http/http-messages.html)
* HTTP URL 编码[REF](https://www.w3ccoo.com/http/http_url_encoding.html)
* HTTP/1.1和HTTP/2.0的请求拆包方式[REF](https://zhuanlan.zhihu.com/p/1893088388950758956)
* 典型的 HTTP 流程[REF](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Guides/Session)
* 手动Http报文解析[](https://www.cnblogs.com/MandyCheng/p/11151803.html)


## 请求消息结构和响应消息结构

HTTP消息分为两种类型：请求消息和响应消息。
    * 请求消息包括以下格式：请求行(request line)、请求头部(header)、空行和请求数据(content)四个部分组成
    * 响应消息包括以下格式：状态行(status line)、响应头部(header)、空行和响应数据(content)四个部分组成

### 请求消息结构

![picture](https://www.runoob.com/wp-content/uploads/2013/11/2012072810301161.png)

* 请求行（Request Line）：
  * 方法：如 GET、POST、PUT、DELETE等，指定要执行的操作。
  * 请求 URI（统一资源标识符）：请求的资源路径，通常包括主机名、端口号（如果非默认）、路径和查询字符串。
  * HTTP 版本：如 HTTP/1.1 或 HTTP/2。
    
  请求行的格式示例：GET /index.html HTTP/1.1

* 请求头（Request Headers）：

  * 包含了客户端环境信息、请求体的大小（如果有）、客户端支持的压缩类型等。常见的请求头包括Host、User-Agent、Accept、Accept-Encoding、Content-Length等。

* 空行：

  *请求头和请求体之间的分隔符，表示请求头的结束。

* 请求体（可选）：
  * 在某些类型的HTTP请求（如 POST 和 PUT）中，请求体包含要发送给服务器的数据。

### 响应消息结构

![picture](https://www.runoob.com/wp-content/uploads/2013/11/httpmessage.jpg)

* 状态行（Status Line）：
  * HTTP 版本：与请求消息中的版本相匹配。
  * 状态码：三位数，表示请求的处理结果，如 200 表示成功，404 表示未找到资源。
  * 状态信息：状态码的简短描述。
  
  状态行的格式示例：HTTP/1.1 200 OK

* 响应头（Response Headers）：
  * 包含了服务器环境信息、响应体的大小、服务器支持的压缩类型等。
  * 常见的响应头包括Content-Type、Content-Length、Server、Set-Cookie等。

* 空行：
  * 响应头和响应体之间的分隔符，表示响应头的结束。

* 响应体（可选）：
  * 包含服务器返回的数据，如请求的网页内容、图片、JSON数据等。

## 请求方法：

各个版本定义的请求方法

**HTTP/1.0**

HTTP/1.0 定义了以下三种请求方法：

* GET - 请求指定的资源。
* POST - 提交数据以处理请求。
* HEAD - 请求资源的响应头信息。

**HTTP/1.1**

HTTP/1.1 引入了更多的方法：

* GET - 请求指定的资源。
* POST - 提交数据以处理请求。
* HEAD - 请求资源的响应头信息。
* PUT - 上传文件或者更新资源。
* DELETE - 删除指定的资源。
* OPTIONS - 请求获取服务器支持的请求方法。
* TRACE - 回显服务器收到的请求，主要用于诊断。
* CONNECT - 建立一个隧道用于代理服务器的通信，通常用于 HTTPS。

**HTTP/2.0,HTTP/3.0沿用了HTTP/1.1的请求方法**

## HTTP格式中必须的字段（来自AI）

### HTTP请求头必需内容

1. 必需：请求行 `GET /index.html HTTP/1.1` 方法+路径+HTTP版本
2. 必需：请求头 `Host: www.example.com` Host头是HTTP/1.1唯一明确要求的头字段。

### HTTP响应头必需内容

1. 必需：响应行 `HTTP/1.1 200 OK` HTTP版本+状态码+状态信息

### 完整的HTTP请求示例：

```http
GET /api/users HTTP/1.1          ← 必需：请求行
Host: api.example.com            ← 必需：Host头
User-Agent: Mozilla/5.0          ← 可选
Accept: application/json         ← 可选但推荐
Content-Type: application/json   ← POST/PUT时推荐
Content-Length: 45               ← 有消息体时必需
Authorization: Bearer token123   ← 需要认证时必需
Connection: keep-alive           ← 可选

{"name": "John", "age": 30}      ← 消息体（可选）
```

### 完整的HTTP响应示例：

```http
HTTP/1.1 200 OK                  ← 必需：状态行
Content-Type: application/json   ← 强烈推荐
Content-Length: 89               ← 有消息体时必需
Date: Mon, 23 Jan 2023 10:30:45 GMT ← 推荐
Server: nginx/1.18.0             ← 可选
Cache-Control: no-cache          ← 可选
Connection: keep-alive           ← 可选

{"status": "success", "data": {...}} ← 消息体（可选）
```

## HTTP可选的字段

1. **内容协商头**

```
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Accept-Charset: utf-8
```

2. 缓存控制头

```
Cache-Control: no-cache
Cache-Control: max-age=3600
Expires: Wed, 21 Oct 2023 07:28:00 GMT
Last-Modified: Mon, 23 Jan 2023 10:30:45 GMT
ETag: "33a64df551425fcc55e4d42a148795d9f25f89d4"
If-Modified-Since: Mon, 23 Jan 2023 10:00:00 GMT
If-None-Match: "33a64df551425fcc55e4d42a148795d9f25f89d4"
```

3. **认证头(cookie)**

```
Authorization: Basic YWxhZGRpbjpvcGVuc2VzYW1l
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
WWW-Authenticate: Basic realm="Access to the staging site"
Cookie: sessionId=abc123; userId=456
Set-Cookie: sessionId=abc123; Expires=Wed, 21 Oct 2023 07:28:00 GMT; HttpOnly
```

4. 4. 条件请求头

```
If-Match: "737060cd8c284d8af7ad3082f209582d"
If-Unmodified-Since: Sat, 29 Oct 1994 19:43:31 GMT
If-Range: "737060cd8c284d8af7ad3082f209582d"
```

5. CORS（跨域）头

```
Origin: https://example.com
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, PUT
Access-Control-Allow-Headers: Content-Type, Authorization
Access-Control-Allow-Credentials: true
```

6. **消息体相关头**

```
http
Content-Type: application/json; charset=utf-8
Content-Length: 348
Content-Encoding: gzip
Content-Disposition: attachment; filename="example.pdf"
Content-Range: bytes 21010-47021/47022
Transfer-Encoding: chunked
```

## 不同方法的经典头部

* **GET**
```
GET /api/products?category=books&page=1 HTTP/1.1
Host: api.example.com
Accept: application/json
User-Agent: MyApp/1.0
```

* **POST**
```
POST /api/users HTTP/1.1
Host: api.example.com
Content-Type: application/json
Content-Length: 45
Authorization: Bearer token123

{"name": "Alice", "email": "alice@example.com"}
```

* **PUT**
```
PUT /api/users/123 HTTP/1.1
Host: api.example.com
Content-Type: application/json
Content-Length: 56
If-Match: "abc123def456"

{"name": "Alice Smith", "email": "alice.smith@example.com"}
```

## 特殊情况下的必须头部

1.  有消息体时的必需头
```
POST /upload HTTP/1.1
Host: example.com
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary
Content-Length: 1024  ← 必需（除非使用Transfer-Encoding: chunked）
```

2. 文件下载响应
```
HTTP/1.1 200 OK
Content-Type: application/pdf
Content-Length: 1048576
Content-Disposition: attachment; filename="document.pdf"  ← 推荐
```

3. 重定向响应
```
HTTP/1.1 302 Found
Location: https://new.example.com/resource  ← 必需用于重定向
```

## 推荐请求和响应头
```
GET /api/data HTTP/1.1
Host: api.example.com
User-Agent: MyApp/1.0
Accept: application/json
Accept-Encoding: gzip, deflate
Connection: keep-alive
```

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 128
Date: Mon, 23 Jan 2023 10:30:45 GMT
Cache-Control: no-cache
X-Content-Type-Options: nosniff
```

🎯 总结
必需内容：

请求：请求行 + Host头

响应：状态行

强烈推荐：

Content-Type（有消息体时）

Content-Length（有固定长度消息体时）

Date（响应）

根据场景必需：

Authorization（需要认证时）

Location（重定向时）

Set-Cookie（设置cookie时）

理解这些头的用途可以帮助你构建更规范、更高效的HTTP通信。