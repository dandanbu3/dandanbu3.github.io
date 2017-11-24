---
layout: post
title: speechSynthesis深度分析
---

在speechSynthesis是一个全局对象，最近用speechSynthesis做了个WEB语音导航，导航方面调用了百度地图的API。speechSynthesis是基于系统层调用的语音合成引擎，所以同一台电脑上，不同浏览器的speechSynthesis对象的语言种类都是一定的。MAC系统的语音合成语言种类会丰富一些,在控制台输入以下语句

~~~
speechSynthesis.getVoices()
~~~
以下是MAC系统firefox输入的结果
![firefox](/img/firefox.png)

以下是MAC系统Chrome输入的结果
![chrome](/img/chrome.png)

发现chrome的语音合成可选择语言达到了66种，而firefox下只有47种，不是说speechSynthesis是基于系统的吗？为何它们在不同浏览器下会有差别？仔细把firefox和chrome下的结果对比，发现speechSynthesis.getVoices()数组前47序号的值基本相同，chrome多的内容，语言名称前都有Google，查看其中的localService参数，发现都是false，这个参数其实是指语音合成是否是本地服务，false表示不是本地服务，这些多的都是Google自己提供的，需要联网才能访问这后面的Google语音合成引擎。
![chrome](/img/google.png)

我们可以仔细看看speechSynthesis.getVoices()数组中的属性名，name指语音合成的名称，lang对应语音合成的语言类型，default指是否是默认语音合成引擎，在使用speechSynthesis.speak()方法时，如果我们不指定语音合成的引擎，则会使用默认的引擎。localService前面也说过，是指是否是本地服务，为false则此语音合成引擎需要联网才能使用。

导航应用基于百度地图API，可以手动输入终点位置，通过百度地图API，获取当前位置的地理位置经纬度信息，生成一条规划路线，并在页面上显示出来。当定位监测到移动时，与规划路线的经纬度对比，并用speechSynthesis.speak()语音合成播放信息。
![map](/img/map.png)

speechSynthesis.speak()除了可以识别文字外，emoji也能识别，比如👵会语音合成“老奶奶”，非常有意思。