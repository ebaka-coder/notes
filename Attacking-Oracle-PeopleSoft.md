### 参考
- [ORACLE PEOPLESOFT REMOTE CODE EXECUTION: BLIND XXE TO SYSTEM SHELL](https://www.ambionics.io/blog/oracle-peoplesoft-xxe-to-rce)
- [exploit-db][RCE vulnerability in monitor service of PeopleSoft 8.54, 8.55, 8.56](https://www.exploit-db.com/exploits/43594)
- [exploit-db][Oracle PeopleSoft Enterprise PeopleTools < 8.55 - Remote Code Execution Via Blind XML External Entity](https://www.exploit-db.com/exploits/43114)
- https://github.com/blazeinfosec/CVE-2017-10366_peoplesoft
- [Oracle PeopleSoft applications are under attacks!](https://conference.hitb.org/hitbsecconf2015ams/wp-content/uploads/2015/02/D1T2-Alexey-Tiurin-Oracle-Peoplesoft-Applications-are-Under-Attack.pdf)


### 原理
- 利用[CVE-2013-3821]`/PSIGW/HttpListeningConnector`的XXE;
- 使用XXE的GET型SSRF的功能;
- 找到Axis服务:`/pspc/services`
- 网络访问Axis服务发现Axis需要认证401，转而尝试使用本地访问Axis
- 访问`/psp/ps/signon.html`，从响应的`Set-Cookie: xxx-80-PORTAL-PSJSESSIONID`中提取出localhost监听的Axis端口号: `80`
- 利用Axis的特性，将本需要用POST请求的部署Service的方式利用一个trick用GET型请求替代，实现了使用XXE(GET型SSRF)部署localhost的Axis的Service的目的
- 使用Axis的copy等指令将webshell转移到指定目录，实现webshell上传
- 访问webshell，实现RCE.


### 杂
- 获取版本号：`/PSEMHUB/hub`
- 找到site name/ID: `/`，匹配其结果`<a href="../ps/signon.html">`，这里就是`ps`

### Demo
