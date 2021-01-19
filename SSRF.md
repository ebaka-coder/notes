## SSRF发现&利用


1、WEB参数点
- url，callback，img等
- HTML/PDF/img的渲染处
- xml、docx、odt等文件（XXE）
- 通过SVG文件上传，参考https://github.com/allanlw/svg-cheatsheet
- 反向代理/负载均衡处：请求头，Host，Referer，X-Forwarded-For


2、重定向
- 重定向到指定Host下（Location: http://127.0.0.1）
- 重定向更改协议（Location: gopher://127.0.0.1:11211/_data）
- 结合重定向漏洞
- 绕过重定向保护（301,302,307等）



3、绕过黑名单保护
### URL解析绕过
- http://evil$google.com，
- http://127.1.1.1:80\@127.2.2.2:80/，
- http://127.1.1.1:80\@@127.2.2.2:80/，
- 0://evil.com:80;http://google.com:80/ 
- Unicode编码
- 地址进制转换（http://0177.0.0.01，http://2130706433/， http://0x7f.0x0.0x0.0x1， http://0177.0x0.0x0.1）
- localhost变换
- IPv6地址变换


4、支持的协议
- ftp（tftp、sftp）
- http（https）
- unc路径
- file、netdoc（Java）文件读取
- jar、dict、gopher、ldap


5、DNS重绑定


6、利用
- 内网扫描（HTTP服务、redis服务）
- 文件读取（依赖协议）
- 绕过403、401等localhost


### 参考
- https://github.com/hackerscrolls/SecurityTips/blob/master/MindMaps/SSRF.png
- [SSRF Tips](https://blog.safebuff.com/2016/07/03/SSRF-Tips/)
- [多个应用的盲SSRF](https://github.com/assetnote/blind-ssrf-chains)
