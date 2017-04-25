--
layout: post
title: 浏览器内置语音发音对象speechSynthesis
--

### 浏览器内置发音speechSynthesis

因为最近做了一个指引功能，然后发现一个比较有意思的东西，以直接通过js调用浏览器内置的speechSynthesis的对象实现语音输出，而不用依赖其他的音频文件。

~~~
var words = new SpeechSynthesisUtterance('Hello captain');
window.speechSynthesis.speak(words);
~~~
通过新建一个SpeechSynthesisUtterance对象，再调用speechSynthesis方法，实现语音输出SpeechSynthesisUtterance对象内容。我在firefox和chrome浏览器下发现是有speechSynthesis对象的，但是IE暂不支持。通过修改SpeechSynthesisUtterance对象中的属性，可以修改语音的声音、发音速度，声调、语言等。

|volume|声音|
|rate|发音速度|
|pitch|音调|
|voice|声音|
|language|[语言en,zh](http://www.mathguide.de/info/tools/languagecode.html)|

~~~
var msg = new SpeechSynthesisUtterance();
var voices = window.speechSynthesis.getVoices();
msg.voice = voices[10]; // 
msg.voiceURI = 'native';
msg.volume = 1; // 0 to 1
msg.rate = 1; // 0.1 to 10
msg.pitch = 2; //0 to 2
msg.text = 'I am Stark';
msg.lang = 'en';

msg.onend = function(e) {
  console.log('Finished in ' + event.elapsedTime + ' seconds.');
};

speechSynthesis.speak(msg);
~~~
#### 发音列表

~~~
speechSynthesis.getVoices().forEach(function(voice) {
  console.log(voice.name, voice.default ? '(default)' :'');
});

~~~
speechSynthesis.getVoices()可以得到可用语言发音数组，forEach方法对数组每一项进行迭代。
可以得到一个列表。

![grep](/img/chromeSpeech.png)

知道这个列表我们就能用指定语言读取内容了

~~~
var msg = new SpeechSynthesisUtterance('你好');
msg.voice = speechSynthesis.getVoices().filter(function(voice) { return voice.name == 'Google 普通话（中国大陆）'; })[0];
speechSynthesis.speak(msg);
~~~
speechSynthesis[浏览器兼容性](https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesis)

也可以直接在控制台输入以下代码进行检测
~~~
if ('speechSynthesis' in window){
//内容
}
~~~
对于不支持的浏览器，可以用html5新加入的audio标签，如果浏览器版本比较低，那就只能用flash实现了。

