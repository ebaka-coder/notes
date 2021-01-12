### 使用@WebFilter注解配置filter
在java-sec-code项目中发现某几个filter并没有在web.xml里注册，
```bash
/d/repos/java-sec-code (master)
$ find .|grep web.xml


```
但是在登录过程的处理中，命中了这几个filter，于是看看是什么原因。找到了类名上方的`@WebFilter`。

参考：
- [@WebFilter 的使用及采坑](https://www.cnblogs.com/muxi0407/p/11950627.html)

示例：
```java
import javax.servlet.annotation.WebFilter;

@WebFilter(filterName = "jsonpFilter", urlPatterns = "/*")
public class JsonpFilter implements Filter {
...
}


@WebFilter(filterName = "referFilter", urlPatterns = "/*")
public class ReferFilter implements Filter {
...
}
```


如此配置之后，就可以不必在 `web.xml` 中配置相应的 <filter> 和 <filter-mapping> 元素了，容器会在部署时根据指定的属性将该类发布为过滤器。它等价的 web.xml 中的配置形式为：
```
<filter> 
　　<filter-name>jsonpFilter</filter-name> 
　　<filter-class>JsonpFilter</filter-class> 
</filter> 
<filter-mapping> 
　　<filter-name>jsonpFilter</filter-name> 
　　<url-pattern>/*</url-pattern>
</filter-mapping>
```


## 参考
