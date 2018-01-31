# vue-qin

> Vue 的琴弦文字插件。

##  Examples
![vue-qin](https://raw.githubusercontent.com/shalldie/vue-qin/master/GIF.gif)

[To see a demo.](https://shalldie.github.io/demos/vue-qin/)

## Installation
    npm install vue-qin

## Usage

```js
import Vue from 'vue';
import VueQin from 'vue-qin';
// let VueQin = window.VueQin.default;  // window
// let VueQin = require('vue-qin').default;  // commonjs
```

```js
// regist 注册组件

Vue.use(VueQin);  // global

// or

new Vue({
    el: 'body',
    components: { VueQin }  // local
});
```

```js
<vue-qin 
    :content="'This is the content to show'"
    :duration="500"
    :recline="0.6"
    :offset="22"
></vue-qin>
```
    
# Api

## Properties

|   Name   |   Type   | Default |                                          Description                                     |
| :------- | :------: | :-----: | :--------------------------------------------------------------------------------------- |
| content  | `String` |  `''`   | The content you want to show.<br>要显示的文字。                                             |
| duration | `Number` |   500   | The animation duration.<br>每次弹动对动画时长。                                              |
| recline  | `Number` |   0.6   | Animation's wave distance between two chars.<br>动画时候2个字符间的波动距离。值越大波动越大。    |
| offset   | `Number` |   22    | Max offset of each char move.<br>字符最大的可移动距离。                                      |

# Enjoy it! :D
