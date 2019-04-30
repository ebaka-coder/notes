两种SSL握手

单向SSL握手（one-way）和双向SSL握手（Mutual SSL）
通常在我们浏览HTTPs网站的时候，只进行了单向SSL握手，因为只需要客户端验证服务端的有效性。而双向SSL出现了双方都需要验证对方身份的情况。

### 1. Client Hello
客户端发送一些信息给服务端，以作为服务端后续开启HTTPs连接使用。
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
```
*** ServerHello, TLSv1.2
RandomCookie: GMT: 1518335451 bytes = { 19, 150, 56, 42, 168, 202, 151, 43, 174, 226, 187, 53, 135, 67, 244, 170, 59, 176, 105, 150, 50, 112, 167, 83, 192, 48, 171, 64 }
Session ID: {91, 128, 246, 219, 26, 93, 46, 172, 85, 212, 221, 79, 20, 186, 108, 134, 200, 239, 150, 102, 172, 24, 125, 171, 137, 53, 5, 130, 53, 228, 2, 195}
Cipher Suite: TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
Compression Method: 0
Extension renegotiation_info, renegotiated_connection: <empty>
***
```


### 参考
https://medium.com/@kasunpdh/ssl-handshake-explained-4dabb87cdce
