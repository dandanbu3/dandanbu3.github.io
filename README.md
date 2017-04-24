## github blog


### let闭包问题

之前做一个项目，用了es6的let去定义变量，没有用babel编译，在firefox和chrome都正常运行，后来测试说在360下无法运行，我也没仔细看代码，就把let直接改成var了。但是发现功能有问题，才发现我在写代码的时候用了闭包，就在嵌套了一个函数，解决闭包引用外部数据的问题。但是在用let定义变量的时候，闭包函数却可以引用外部变量，感到十分好奇。所以找资料研究了一下。(http://www.cnblogs.com/zhuzhenwei918/p/6131345.html)这篇博客解释的不错。

###浏览器内置发音speechSynthesis
因为最近做了一个指引功能，然后发现一个比较有意思的东西，浏览器内置的speechSynthesis的对象可以直接通过js调用实现语音输出，而不用依赖其他的音频文件。
// 你可以直接打开你的控制台粘贴下面代码
var words = new SpeechSynthesisUtterance('Hello captain');
window.speechSynthesis.speak(words);
通过新建一个SpeechSynthesisUtterance对象，再调用speechSynthesis方法，实现语音输出SpeechSynthesisUtterance对象内容。我在firefox和chrome浏览器下发现是有speechSynthesis对象的，但是IE暂不支持。通过修改SpeechSynthesisUtterance对象中的属性，可以去修改语音的声音、发音速度，声调、语言等。

