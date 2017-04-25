--- 
layout: post
title: 浏览器内置语音发音对象speechSynthesis
---

### 浏览器内置发音speechSynthesis

因为最近做了一个指引功能，然后发现一个比较有意思的东西，浏览器内置的speechSynthesis的对象可以直接通过js调用实现语音输出，而不用依赖其他的音频文件。

~~~
// 你可以直接打开你的控制台粘贴下面代码
var words = new SpeechSynthesisUtterance('Hello captain');
window.speechSynthesis.speak(words);
~~~
通过新建一个SpeechSynthesisUtterance对象，再调用speechSynthesis方法，实现语音输出SpeechSynthesisUtterance对象内容。我在firefox和chrome浏览器下发现是有speechSynthesis对象的，但是IE暂不支持。通过修改SpeechSynthesisUtterance对象中的属性，可以去修改语音的声音、发音速度，声调、语言等。

