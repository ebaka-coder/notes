两种SSL握手

单向SSL握手（one-way）和双向SSL握手（Mutual SSL）
通常在我们浏览HTTPs网站的时候，只进行了单向SSL握手，因为只需要客户端验证服务端的有效性。而双向SSL出现了双方都需要验证对方身份的情况。

### 1. Client Hello
客户端发送一些信息给服务端，以作为服务端后续开启HTTPs连接使用。
- SSL/TLS版本号
- random byte string（用于后续的计算）
```
*** ClientHello, TLSv1.2
RandomCookie: *** ClientHello, TLSv1.2
RandomCookie: GMT: -1892413556 bytes = { GMT: -351008774 bytes = { 169, 131, 204, 213, 154, 96, 7, 136, 43, 142, 232, 138, 148, 171, 52, 226, 155, 202, 145, 57, 210, 132, 227, 182, 67, 222, 161, 28, 20 }
Session ID: 239, 10, 92, 143, 185, {}
93, Cipher Suites: [Unknown 0x8a:0x8a, TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256, TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256, TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384, TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384, Unknown 0xcc:0xa9, Unknown 0xcc:0xa8, TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA, TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA, TLS_RSA_WITH_AES_128_GCM_SHA256, TLS_RSA_WITH_AES_256_GCM_SHA384, TLS_RSA_WITH_AES_128_CBC_SHA, TLS_RSA_WITH_AES_256_CBC_SHA, SSL_RSA_WITH_3DES_EDE_CBC_SHA]
………………………………………………
```
其中`TLS v1.2`表示客户端只支持TLS v1.2及以下TLS版本。

### 2. Server Hello
- 服务端发送从客户端收到的CipherSuite 列表中选出来的CipherSuite ，然后服务端还发送自身的证书；
- 另一个random byte string
- 如果服务端有验证客户端证书的需求，它会发送`client certificate request`消息给客户端
```
*** ServerHello, TLSv1.2
RandomCookie: GMT: 1518335451 bytes = { 19, 150, 56, 42, 168, 202, 151, 43, 174, 226, 187, 53, 135, 67, 244, 170, 59, 176, 105, 150, 50, 112, 167, 83, 192, 48, 171, 64 }
Session ID: {91, 128, 246, 219, 26, 93, 46, 172, 85, 212, 221, 79, 20, 186, 108, 134, 200, 239, 150, 102, 172, 24, 125, 171, 137, 53, 5, 130, 53, 228, 2, 195}
Cipher Suite: TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
Compression Method: 0
Extension renegotiation_info, renegotiated_connection: <empty>
***
```

### 3. 
Client验证Server的证书，细节参考：https://www.ibm.com/support/knowledgecenter/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10670_.htm

### 4. 
Client使用Server's 公钥加密生成一个random byte string，然后将其发送给Server（之后将用于对密钥进行计算，加密后续传输的数据）。

### 5.
若Server发送了`client certificate request`消息，Client收到后，会使用Client's私钥加密生成一个random byte string，连同Client's证书发送给Server。或者发送`no digital certificate alert`，这个alert只是一个警告，在某些协议的实现中，如果Client的认证是必需的，这样会导致握手失败。

### 6.
Server验证Client的证书，详情参考：https://www.ibm.com/support/knowledgecenter/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10670_.htm

### 7.
若Client使用密钥向Server发送`finished`消息，表明Client这边的握手已经完成

### 8.
若Server使用密钥向Client发送`finished`消息，表明Server这边的握手已经完成

### 9.
在接下来的SSL/TLS会话中，Server和Client就可以通过使用共享密钥（shared secret key）对称加密的方式进行通信了。

### 图
![](https://www.ibm.com/support/knowledgecenter/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10660a.gif)

### 参考
https://medium.com/@kasunpdh/ssl-handshake-explained-4dabb87cdce
https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10660_.htm
