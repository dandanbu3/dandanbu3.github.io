---
layout: post
title: vue中eslint设置
---

[eslint中文网站](http://eslint.cn/)

[代码规范](https://github.com/bilibili-fe/spec)

好久没写过博客了，现在想重拾过去的在习惯，这篇博客主要是想记录一下开发过程中的代码规范。不管是大的或者小的公司，对代码的可读性都会有所要求，规范的代码看起来赏心悦目，乱七八糟的代码看起来就不想维护了，特别对我这种强迫症来说，所以在引入代码规范就显得比较重要了。上面的代码规范是目前所在部门的开发代码规范，我觉得比较好用，而且与eslint的基础代码规范是符合的。现在的代码规范插件其实不少，选择eslint一个是因为平常用得比较多，一个是vue-cli项目默认集成的代码规范组件就是eslint了。那么如果已经搭建好的项目中，如何引入eslint呢？

安装过程中，有两种方式，一种全局安装，一种在本项目安装
~~~
// 全局安装
npm install -g eslint

// 在项目目录下输入
eslint --init
~~~

如上步骤允许后，选择eslint的参数，就可以在项目目录下看到文件名为eslintrc的文件（格式可能是js或者json，安装的时候可选）。

~~~
// 项目内安装
npm install --save -D eslint

//
./node_modules/.bin/eslint --init
~~~

eslint初始化之后，生成的eslintrc文件中的内容比较简单

~~~
module.exports = {
  extends: 'standard'
}
~~~

拿vue-cli中的eslint配置对比

~~~
module.exports = {
  root: true,
  parser: 'babel-eslint',
  parserOptions: {
    sourceType: 'module'
  },
  env: {
    browser: true,
  },
  // https://github.com/standard/standard/blob/master/docs/RULES-en.md
  extends: 'standard',
  // required to lint *.vue files
  plugins: [
    'html'
  ],
  // add your custom rules here
  'rules': {
    'space-before-function-paren': 0,
    'semi': [2, 'always'],
    // allow paren-less arrow functions
    'arrow-parens': 0,
    'import/extensions': [2, 'always', {
      'js': 'never',
      'vue': 'never'
    }],
    'eol-last': 0,
    'indent': 0,
    'no-trailing-spaces': 0,
    'object-property-newline': 0,
    'comma-dangle': 0,
    'import/no-unresolved': [2],
    // allow async-await
    'generator-star-spacing': 0,
    // allow debugger during development
    'no-debugger': process.env.NODE_ENV === 'production' ? 2 : 0,
    'camelcase': 0,
  }
}
~~~

root属性是说明此文件是eslint的根目录配置文件，一般情况下ESLINT会在所有父级目录查找配置文件，一直到根目录，设置root为“true”，它会停止在父级目录寻找。env属性是指eslint的运行环境，一般可以设置node或者brower。


安装eslint之后，需要在webpack下进行配置，没有引用的话也不会在项目内生效，vue项目的配置如下，对项目内的js或者vue文件使用eslint检查

~~~
// webpack中rules配置
{
    test: /\.(js|vue)$/,
    loader: 'eslint-loader',
    enforce: 'pre',
    include: [resolve('src'), resolve('test')],
    options: {
      formatter: require('eslint-friendly-formatter')
    }
}
~~~

在这中间我们引入了‘eslint-friendly-formatter’模块，所以要手动安装此模块，此模块可以让eslint错误显示在浏览器页面上，方便调试

~~~
npm install --save -D eslint-friendly-formatter
~~~

除了eslintrc文件，我们可以在同目录下设置一个eslintignore文件，此文件可以设置eslint忽略检查文件，一些开发配置的代码格式就无需对其进行检查，一般我们设置的内容如下

~~~
/build/
/config/
/dist/
/*.js
~~~

经过上述设置，一个具有eslint的vue项目就集成了，可以愉快地使用eslint来规范我们的代码了。
