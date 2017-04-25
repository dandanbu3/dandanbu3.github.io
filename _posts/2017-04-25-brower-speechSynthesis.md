---
layout: post
title: Flake it till you make it
subtitle: Excerpt from Soulshaping by Jeff Brown
bigimg: /img/path.jpg
---


��Ϊ�������һ��ָ�����ܣ�Ȼ����һ���Ƚ�����˼�Ķ�������ֱ��ͨ��js������������õ�speechSynthesis�Ķ���ʵ�����������������������������Ƶ�ļ���

~~~
var words = new SpeechSynthesisUtterance('Hello captain');
window.speechSynthesis.speak(words);
~~~
ͨ���½�һ��SpeechSynthesisUtterance�����ٵ���speechSynthesis������ʵ���������SpeechSynthesisUtterance�������ݡ�����firefox��chrome������·�������speechSynthesis����ģ�����IE�ݲ�֧�֡�ͨ���޸�SpeechSynthesisUtterance�����е����ԣ������޸������������������ٶȣ����������Եȡ�

|volume|����|
|rate|�����ٶ�|
|pitch|����|
|voice|����|
|language|[����en,zh](http://www.mathguide.de/info/tools/languagecode.html)|


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
#### �����б�

~~~
speechSynthesis.getVoices().forEach(function(voice) {
  console.log(voice.name, voice.default ? '(default)' :'');
});

~~~
speechSynthesis.getVoices()���Եõ��������Է������飬forEach����������ÿһ����е�����
���Եõ�һ���б�

![grep](/img/chromeSpeech.png)

֪������б����Ǿ�����ָ�����Զ�ȡ������

~~~
var msg = new SpeechSynthesisUtterance('���');
msg.voice = speechSynthesis.getVoices().filter(function(voice) { return voice.name == 'Google ��ͨ�����й���½��'; })[0];
speechSynthesis.speak(msg);
~~~
speechSynthesis[�����������](https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesis)

Ҳ����ֱ���ڿ���̨�������´�����м��
~~~
if ('speechSynthesis' in window){
//����
}
~~~
���ڲ�֧�ֵ��������������html5�¼����audio��ǩ�����������汾�Ƚϵͣ��Ǿ�ֻ����flashʵ���ˡ�
