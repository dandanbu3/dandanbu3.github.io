---
layout: post
title: emoji与unicode编码
---

我们在接触emoji之后，知道他们的编码unicode码一般在U1+0000至U1FFFFF之间。结合以前的charCodeAt方法，可以返回指定位置的字符的 Unicode 编码，这个返回值是 0 - 65535 之间的整数。

~~~
('😀').charCodeAt(0)
// 55357
~~~

这里难免会有疑问，emoji不是属于U1+0000之后的编码吗，为何会返回55357？在这里，emoji的编码已经采用四字节编码，而charCodeAt返回值的范围是 0 - 65535， 我们再次对emoji进行charCodeAt，

~~~
('😀').charCodeAt(1)
// 56832
~~~

这样，可以获取一个emoji的完整编码，连接起来'\ud83d\ude00'，这种编码方式是utf-16的形式

~~~
console.log('\ud83d\ude00')
// 😀
~~~

fromCharCode可以接受一个unicode值，然后返回一个字符串，这样，我们知道emoji的unicode编码同样可以生成emoji字符，笑脸的unicode编码是U+1F601，换算成数字是128513

~~~
String.fromCharCode(65)
// A

String.fromCharCode(128513)
// 
~~~
发现fromCharCode函数并不能识别，查阅MDN，fromCharCode函数并不能识别65535以上的数字，各浏览器有新的函数fromCodePoint可以识别65535之上编码的字符，

~~~
String.fromCodePoint(128513)
// 😁
~~~
