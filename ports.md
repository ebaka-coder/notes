## 参考
- https://mp.weixin.qq.com/s/N7mJUTGFz9arA7_tyy2M2A
- https://mp.weixin.qq.com/s/bVB2-tebmXR7SuWaXOUUfw


## 收集各种端口号
```
- 1099/1090: RMI(反序列化)
- 11211: memcached（未授权访问）
- 1433: SQLServer
- 1521: Oracle数据库（爆破）
- 16010: hbase
- 2049: nfs(敏感信息泄露)
- 2181: zookeeper(未授权访问)
- 2375: docker（未授权访问）
- 27017/27018: mongodb（未授权访问）
- 3306: mysql（弱口令）
- 3389: RDP（远程桌面）
- 389: ldap（注入、未授权访问）
- 4100: SysBase(爆破、RCE)
- 4242: opentsdb
- 443: SSL/TLS(heartbleed)
- 4848: glassfish（爆破、控制台弱口令、认证绕过）
- 50000: SAP（RCE）
- 5000: sybase/DB2（爆破、注入）
- 50070/50010: hadoop（未授权访问）
- 5432: postgres
- 5432: Postgresql
- 5900: VNC
- 5984: CouchDB
- 6379: redis
- 7001/7002: weblogic（t3、HTTP、IIOP）
- 8000: jdwp(RCE)
- 8083/8086: influxDB（未授权访问）
- 8069: Zabbix（RCE）
- 8080: tomcat、jboss等Java WEB中间件
- 8161: ActiveMQ（文件上传、反序列化）
- 873: rsync（未授权访问）
- 8983: Solr(RCE)
- 9000: fastcgi(RCE)
- 9001: supervisor（RCE）
- 9090: websphere（反序列化、爆破）
- 9200/9300: elasticsearch（未授权访问）
- 9411: zipkin
```
