# 准备：网络通信

## 1. DNS

* 浏览器缓存，系统缓存，路由器缓存，本地域名服务器缓存，根域名服务器，顶级域名服务器，主域名服务器，保存结果至缓存。
* 递归：只发一次请求，要求对方给出最终结果。例如主机向本地域名服务器的查询。
* 迭代：发出一次请求，对方如果没有答案，它就会返回一个能解答这个查询的其它名称服务器列表。本地域名服务器向根域名服务器的查询的查询。

## 2. 从输入url到网页显示的过程中发生了什么？

1. DNS查询
2. TCP握手
3. TLS握手
4. 请求资源：200继续解析；300重定向；400客户端错误；500服务端错误
5. HTML解析器将html文档解析为DOM树。
6. 解析CSS，构建CSSOM
7. 将DOM和cssom合并为渲染树
8. 渲染树布局
9. 渲染树绘制   

## 3. HTTPS 是如何实现安全性的

1. **加密\(Encryption\)**， HTTPS 通过对数据加密来使其免受窃听者对数据的监听，这就意味着当用户在浏览网站时，没有人能够监听他和网站之间的信息交换，或者跟踪用户的活动，访问记录等，从而窃取用户信息。      RSA 的运算速度非常慢，而 AES 的加密速度比较快， TLS 使用了混合加密方式。在通信刚开始的时候使用非对称算法，首先解决密钥交换的问题。然后用随机数产生对称算法使用的会话密钥（session key），再用公钥加密。对方拿到密文后用私钥解密，取出会话密钥。这样，双方就实现了对称密钥的安全交换。
2. **数据一致性\(Data integrity\)**，数据在传输的过程中不会被窃听者所修改，用户发送的数据会完整的传输到服务端，保证用户发的是什么，服务器接收的就是什么。 在 TLS 中，实现完整性的手段主要是**摘要算法**\(Digest Algorithm\)。
3. **身份认证\(Authentication\)**，是指确认对方的真实身份，也就是证明你是你（可以比作人脸识别） ，它可以防止中间人攻击并建立用户信任。
4. 颁发数字证书解决公钥的信任问题，客户端可以验证服务端的数字证书的真实性，如果是真实的，使用该公钥加密对称加密密钥，发给服务端，服务端使用私钥解密，即可完成公钥协商。如果没有证书，那么黑客有可能伪装成服务端发给用户一个公钥，从而与客户端达成连接。黑客的目的就是作为转发站插在客户端和服务端通信的中间。
5. 参考：[https://www.cnblogs.com/cxuanBlog/p/12490862.html](https://www.cnblogs.com/cxuanBlog/p/12490862.html) [https://blog.csdn.net/qq\_38289815/article/details/107591115](https://blog.csdn.net/qq_38289815/article/details/107591115)

## 4. HTTP 请求

### AJAX 

ajax 是异步http 请求，它的核心是 JavaScript 的 XMLHttpRequest 对象。

```text
function ajax(options){
    let method,url,datatype,async,data;
    ...//从 options 中获取 ajax 的参数
 
    return new Promise(function(reslove,reject){
       const xhr=new XMLHttpRequest();
       //set properties of request object
       xhr.ontimeout=()=>{}
       xhr.onstatereadychange=()=>{
          ...
          if(readyState===4 && ((status>=200 && status<300) || status==304)){
             reslove(xhr.response)
          }else{
             reject()
          }
       }
       xhr.onerror=(err)=>{
          ...
          reject(err)
       }
       
       keys=Object.keys(data)
       list=keys.map((key)=>{
          return key+'='+data[key]
       })
       encodeData=list.join('&')
       
       if(method==='GET'){
          //add data at the end of url
       }
       xhr.open(url,method,async);
       if(method=='GET'){
          xhr.send(null);
       }else if(method=='POST'){
          xhr.setRequestHeader("Content-type", "application/x-www-form-urlencoded"); 
          xhr.send(data)
       }
    })
```

### GET 和 POST 的区别

1. 参考：99%的人都理解错了 HTTP 中 GET 与 POST 的区别  [https://learnku.com/articles/25881](https://learnku.com/articles/25881)
2. 这个文章揭开了 HTTP 请求类型的本质，但我还没打算细看。

### 响应码

1. 信息响应 100：continue
2. 成功响应  
   200：OK

   204：OK 且无响应内容

3. 重定向 300：multiple choice 301:   move permanently 永久性转移 302：move Temporarily 暂时性转移 304：未修改。自从上次请求至今资源没有变动（客户端上传last modified time），不返回内容。
4. 客户端响应 400：Bad request 403：Forbidden ****404：Not Found
5. 服务端响应 500：Internal service error 502：Bad Gateway 503：Service Unavliable

## 6. 网络安全

### CSRF 攻击

CSRF（cross site request forgery）跨站请求伪造攻击。攻击者通过引导某一网站的使用者进入第三方网站，向攻击网站发送跨站请求，利用用户已经获得的登录注册凭证，绕过后台验证，达到冒充用户对被攻击的网站执行某项操作的目的。  
参考：[https://tech.meituan.com/2018/10/11/fe-security-csrf.html](https://tech.meituan.com/2018/10/11/fe-security-csrf.html)

* CSRF（通常）发生在第三方网站
* CSRF攻击者不能获取到Cookie等信息，只是使用。
* 防护策略
  * 阻止不明外域的访问
    * 同源检测 √
    * samesite cookie ？
  * 提交时附加本域才能获取的信息
    * CSRF token ？
    * 双重 cookie 验证 ？

### XSS 攻击

1. Cross-Site Scripting（跨站脚本攻击）简称 XSS，是一种代码注入攻击。攻击者通过在目标网站上注入恶意脚本，使之在用户的浏览器上运行。利用这些恶意脚本，攻击者可获取用户的敏感信息如 Cookie、SessionID 等，进而危害数据安全。
2. 向 DOM 注入script代码实现攻击，通常使用HTML转义解决。对于页面中所有用户输入的内容，都要进行HTML转义。
3. 参考：[https://tech.meituan.com/2018/09/27/fe-security.html](https://tech.meituan.com/2018/09/27/fe-security.html)

## 8. 跨域方案

### 同源检测

1. 同源：协议、域名、端口完全相同
2. 同源策略：一种简单的防范CSRF攻击的方法，由浏览器执行，主要规则有：
   1. 无法用js读取非同源的Cookie、LocalStorage 和 IndexDB 无法读取。 
   2. 无法用js获取非同源的DOM。 
   3. 无法用js发送非同源的AJAX请求 。

### 同源策略限制下 http 请求的正确打开方式

1. **CORS 跨域资源共享**
   1. 在HTTP协议中，每一个异步请求都会带上两个header，用于标记来源域名，分别是origin和referer。这个字段是由浏览器添加的，不能由前端自定义修改，这很重要。
   2. 后端收到请求之后，对origin字段进行校验，如果后端在CORS白名单中没有找到该地址，则发送不包含Access-Control-Allow-Origin字段的响应报文，表示不允许请求资源。如果没有使用django 等框架，那后端接口要主动判断，在响应中主动添加这一字段。
   3. 值得注意的是，这种情况下响应码仍然是200，因为服务端确实响应了报文，不过当前端识别不到Access-Control-Allow-Origin字段后，会在控制台报错。这种情况下，即使服务端返回body 中带有内容，前端也不会解析。 我在实验中发现，如果服务端返回的 ACAO 字段有两个或以上 ip，前端同样会报错。因此，CORS跨域要求服务端返回字段，该字段明确指向请求方的origin 地址（需要带有https://），或是\*
   4. 在阻止外域请求时，为了网页的初次展示，服务器往往会过滤掉页面请求。相应的，页面请求暴露在攻击范围内。如果使用GET请求实现产品功能，同样会受到CSRF攻击。（存疑）
   5. 对于简单请求，只需要在请求时附上origin、referer等字段；对于复杂请求，则需要先发送一个预请求，服务端返回access-control-allow-xxx 字段，并设置max-age ，在这段时间内都不需要预请求。[http://www.ruanyifeng.com/blog/2016/04/cors.html](http://www.ruanyifeng.com/blog/2016/04/cors.html)
2. **JSONP 跨域**
   1. 利用浏览器不限制script标签跨域请求的特性，把请求设计为script标签添加到 head 中，如 `<script> function f(data){alert(data)} </script>`  `<script src='http://localhost:4000?callback=f' />` 服务端要配合这一写法，返回 f\('hello world'\) 字段，这样 script 就会执行f 函数。 当然，jquery 也对此方法进行了封装，所以可以用 ajax 来请求，把dataType 设为 ”jsonp“，使用了ajax 就可以直接在success 声明回调函数了。当然也可以用原来的方法，将jsonp 属性设为 callback，将jsonpcallback 属性设为函数名。
   2. 只能发送get请求。//个人认为，黑客无法从JSONP进行CSRF攻击。
   3. JSONP 是可以在 JavaScript 中封装为一个方法的，像ajax 就有method ：jsonp。封装的大致思路是，create element &lt;script&gt;，设置元素的 src；在方法内设置回调函数 callback，并将其绑定在window 对象下。ajax 是使用promise 写的，所以可以将获取到的data 通过resolve 传递出来，并且在回调之后，将script 标签删除，将回调删除，这是因为http 请求是一次性的。
   4. 参考：[https://segmentfault.com/a/1190000015597029](https://segmentfault.com/a/1190000015597029) 
3. **空 iframe 加 form**
   1. 据我实验，这个方法的思路是，用form 发送post 请求，但是form 元素在提交表单时会跳转到 action 属性指定的页面，并且提交post 请求。如果将form 元素加在 iframe 元素内部，就不会发生跳转，或者说，跳转发生在iframe 内部，不影响全局页面，只需要将iframe 设为 display：none就可以了。
4. nginx 代理
5. websocket 双工通信

### 同源策略限制下 DOM 访问的正确打开方式（没搞懂）

1. iframe + domain
2. iframe + location.hash 
3. iframe + window.name
4. postMessage

参考：[https://juejin.cn/post/6954610030347812878\#heading-8](https://juejin.cn/post/6954610030347812878#heading-8)  
本来已经整理完了，但是没保存。

## HTTP 各个版本

### HTTP 1.0

1. 默认短连接
2. 基于TCP
3. 发出一个请求之后，等待响应后才能继续发送下一个请求。
4. 使用文本格式

### HTTP 1.1

1. 默认长连接
2. 管线化技术：同一个TCP连接可同时发出多个HTTP请求，但只能按照发送顺序来接受响应，这要求服务端按顺序处理请求，按顺序返回，客户端无法识别。
3. 一个新的http请求既可以加入tcp队列，也可以新建一个tcp连接。
4. 浏览器在同一时间对同一域名的TCP连接的数量受限制，一个独立的用户最多建立2个TCP连接，代理主机可以建立2\*N个TCP连接（N是使用代理的用户数量），超过限制数量的请求会被阻塞。这与服务器的链接压力有关。为绕开这一限制，许多网站会设置多个静态CDN资源域名。

### HTTP2.0

1. 多路复用：允许在同一TCP连接中同时发起多个请求，解决队头阻塞。
2. 二进制分帧：在保持http语义不变的情况下，在http和tcp之间通过binary framing层将报文进行二进制编码并分为多个更小的frame。这使得所有通信都以帧在形式在同一个tcp连接中传输，在以下两方面提高了性能。
   1. TCP有一个”慢启动“的机制，在TCP连接起初限制速度，如果通信成功，会随着时间增加逐步增大速度。http2之前，例如1.0的短连接，http的突发性、短时性加上tcp的慢启动，大大影响了性能。而http2开始，所有数据流共用一个连接，tcp利用率高，不会多次”慢启动“，速度逐步增快之后，真正利用了高带宽。
   2. TCP 连接数变少使得服务端链接压力减小，内存占用小，连接吞吐量更大。
3. 首部压缩

   对于同样的报文头部，不必要在每次请求和响应都发送，只需附上更改的或是新增的部分即可。

4. 服务端推送

   服务端在用户请求某个资源后，判断其需要另一份资源，主动向客户端推送。例如在用户请求html页面时，服务端随后推送js和css。

5. 请求优先级

   客户端在发送请求时可以指定优先级，让服务端根据优先级来设置响应顺序。

6. 流量控制

   网上说的感觉不太理解，我感觉和TCP的窗口类似吧，因为帧需要重组，未完成重组的需要缓存。

## TCP

### 三次握手

1. 过程： A-&gt;B \[SYN=1 SEQ=0\] B-&gt;A \[SYN=1 SEQ=0 ACK=1\] A-&gt;B \[SEQ=1 ACK=1\]
2. 重传机制：
   1. 携带数据的segment在一段时间内没有收到ack时，会触发重传机制。
   2. 在握手阶段，携带SYN字段的segment相当于有一个字节的数据包，因此前两次握手都有超时重传机制。
   3. 我觉得第三次之所以没有携带数据，是因为他不需要ack，如果第三次也需要ack，那必然有第四次，那就无穷无尽了。 
3. 必要性：
   1. 三次重传可以保证双方都拿到对方的序列码，第一次丢失A重传，第二次丢失A或B重传，第三次丢失B重传。
   2. 如果只有两次，双方可能没有对B的序列码达成一致，那么此时由B发往A的包就是不可靠的；相对的，A的序列码已经达成一致，A是可以放心往B发送segment的。

### 四次挥手

1. 和上面的道理一样，第一次和第三次都有FIN码，二四没有，因为他们不需要acknowledge。第四次挥手的必要性就体现在，如果第三个包丢失，服务端会重传，如果第四个包丢失，服务端依然重传。如果只有三次挥手，没人需要ack，那服务端悄悄下线，客户端收没收到也不知道。
2. 客户端等待两个最长报文寿命是因为，当第四个包丢失后，服务端会再重发FIN，如果这个时候客户端下线就无法响应了，所以等待两个最长报文寿命。



