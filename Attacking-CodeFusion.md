## 参考

- [ColdFusion for Pentesters](http://www.carnal0wnage.com/papers/LARES-ColdFusion.pdf)
- http://web.archive.org/web/20101226181618/http://www.security-assessment.com/files/advisories/2010-02-22_Multiple_Adobe_Products-XML_External_Entity_and_XML_Injection.pdf


### Google Dorks
```
- filetype:cfm "cfapplication name" password
- inurl:login.cfm
- intitle:"Error Occurred" "The error occurred in"
- filetype:cfm
- intitle:"ColdFusion Administrator Login“
- intitle:"Index of" cfide
- inurl:/CFIDE/componentutils/
```


### CodeFusion各个版本的界面


## 利用工具
- https://github.com/codewhitesec/ColdFusionPwn/tree/master/src/main/java
- https://github.com/mbechler/marshalsec/blob/6e5f2a7974a62fcd0e5bbbf8df64774a50db2eb8/src/main/java/marshalsec/BlazeDSAMF3AM.java
- https://github.com/mbechler/marshalsec/blob/0471b932a09c8aca21876de80c8abf65b251c9ca/src/main/java/marshalsec/BlazeDSAMFX.java
- https://github.com/vulhub/vulhub/blob/master/coldfusion/CVE-2017-3066/README.zh-cn.md
