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

这样，可以获取一个emoji的完整编码，连接起来'\ud83d\ude00'，这种编码方式是UTF-16的形式

~~~
console.log('\ud83d\ude00')
// 😀
~~~

为何在js中只能用UTF-16的编码表示emoji，我们不是一般页面编码是UTF-8吗？这就要追溯到js产生的背景了，js诞生于1995年，那个时候并没有UTF-8或者UTF-16编码，当时js使用的是UCS-2，后来UTF-16收录了UCS-2，也就是说UCS-2是UTF-16的子集，UCS-2是一种以2字节的编码方式。这也就是为什么emoji包含两个字符了，在emoji的编码中，运用了一些代理码位，范围从D800-DFFF，只要编码第一个字符在这个范围内，就认为是由两个字符连接起来的。

由于之前写法的不兼容，es6已经修复了这种问题，现在超过UFFFF的编码，也可以用下面的方式

~~~
console.log('\u{1F600}')
// 😀
~~~

现在，有一个Unicode转UTF-16的转码公式，其中c为Unicode编码
~~~
H = Math.floor((c-0x10000) / 0x400)+0xD800

L = (c - 0x10000) % 0x400 + 0xDC00
~~~

~~~
H = Math.floor((0x1F600-0x10000) / 0x400)+0xD800 = 0xD83D
L = (0x1F600 - 0x10000) % 0x400 + 0xDC00 = 0xDE00
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
