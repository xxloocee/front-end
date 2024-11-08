# 前端手册
对于html,css,js基础语法并不给出详细文档，只给出个人认为比较好的学习资料和资源的链接，持续完善中
## javascript
  + [js教程-网道](https://wangdoc.com/javascript/index.html)
  + [es6入门-阮一峰](http://es6.ruanyifeng.com/)
  + [js模块化](https://juejin.im/post/5d348134f265da1b904c1e0c)
### js技巧
### js优质代码
### js归纳总结
  + 遍历语法
  1. 传统for循环：
    写法比较麻烦
  2. 数组内置forEach：
    缺点在于中途无法跳出循环，break和return都无法奏效
  3. for ... in：
    可以遍历数组的键名，对于普通的对象，for...in循环可以遍历键名，for...in循环主要是为遍历对象而设计的，不适用于遍历数组，缺点是
    - 数组的键名是数字，但是for...in循环是以字符串作为键名“0”、“1”、“2”等
    - for...in循环不仅遍历数字键名，还会遍历手动添加的其他键，甚至包括原型链上的键
    - 某些情况下，for...in循环会以任意顺序遍历键名
  4. for ... of：
  for...of循环，作为遍历所有数据结构的统一的方法，一个数据结构只要部署了Symbol.iterator属性，就被视为具有 iterator 接口，就可以用for...of循环遍历它的成员，for...of循环可以使用的范围包括数组、Set 和 Map 结构、某些类似数组的对象（比如arguments对象、DOM NodeList 对象）、Generator 对象，以及字符串，但对于普通的对象，for...of结构不能直接使用，会报错，必须部署了 Iterator 接口后才能使用，for...of循环可以代替数组实例的forEach方法，返回键值，优点是
  - 有着同for...in一样的简洁语法，但是没有for...in那些缺点
  - 不同于forEach方法，它可以与break、continue和return配合使用
  - 提供了遍历所有数据结构的统一操作接口
### js常用代码片段
```javascript
// 求数组最大元素

// ES5 的写法
Math.max.apply(null, [14, 3, 77])
// ES6 的写法
Math.max(...[14, 3, 77])
```
```javascript
// 通过push函数，将一个数组添加到另一个数组的尾部

// ES5的 写法
let arr1 = [0, 1, 2];
let arr2 = [3, 4, 5];
Array.prototype.push.apply(arr1, arr2);
// ES6 的写法
let arr1 = [0, 1, 2];
let arr2 = [3, 4, 5];
arr1.push(...arr2);
```
```javascript
// 复制数组
// ES5
let a1 = [1, 2];
let a2 = a1.concat();
// ES6
let a1 = [1, 2];
let a2 = [...a1];
```
```javascript
// 合并数组，这两种方法都是浅拷贝
// ES5
arr1.concat(arr2, arr3);
// ES6
[...arr1, ...arr2, ...arr3]
```
```javascript
// JavaScript 会将四个字节的 Unicode 字符，识别为 2 个字符，采用扩展运算符就没有这个问题,正确返回字符串长度如下
function length(str) {
  return [...str].length;
}
// 凡是涉及到操作四个字节的 Unicode 字符的函数，都有这个问题。因此，最好都用扩展运算符改写
let str = 'x\uD83D\uDE80y';
str.split('').reverse().join('')
// 'y\uDE80\uD83Dx'
// 上面代码中，如果不用扩展运算符，字符串的reverse操作就不正确
[...str].reverse().join('')
// 'y\uD83D\uDE80x'
```
```javascript
// 一个类似数组的对象，转为真正的数组,对于还没有部署该方法的浏览器，可以用Array.prototype.slice方法替代
const toArray = (() =>
  Array.from ? Array.from : obj => [].slice.call(obj)
)();
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};
// ES5的写法
var arr1 = [].slice.call(arrayLike); // ['a', 'b', 'c']
// ES6的写法
let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']
// NodeList对象
let ps = document.querySelectorAll('p');
Array.from(ps).filter(p => {
  return p.textContent.length > 100;
});
// arguments对象
function foo() {
  var args = Array.from(arguments);
  // ...
}
// 扩展运算符背后调用的是遍历器接口（Symbol.iterator），如果一个对象没有部署这个接口，就无法转换
// NodeList对象
[...document.querySelectorAll('div')]
// arguments对象
function foo() {
  const args = [...arguments];
}
// 任何有length属性的对象，都可以通过Array.from方法转为数组,而此时扩展运算符就无法转换
```
```javascript
// 取出一组 DOM 节点的文本内容
let spans = document.querySelectorAll('span.name');
// map()
let names1 = Array.prototype.map.call(spans, s => s.textContent);
// Array.from()
let names2 = Array.from(spans, s => s.textContent)
```
```javascript
// 将数组中布尔值为false的成员转为0
Array.from([1, , 2, , 3], (n) => n || 0)
// [1, 0, 2, 0, 3]
```
```javascript
// 返回各种数据的类型
function typesOf () {
  return Array.from(arguments, value => typeof value)
}
typesOf(null, [], NaN)
// ['object', 'object', 'number']
```
```javascript
// Array.from()可以将各种值转为真正的数组，并且还提供map功能
Array.from({ length: 2 }, () => 'jack')
// ['jack', 'jack']
```
```javascript
// Array.of方法可以用下面的代码模拟实现
function ArrayOf(){
  return [].slice.call(arguments);
}
```
```javascript
// 识别数组中的NaN
[NaN].indexOf(NaN)
// -1
[NaN].findIndex(y => Object.is(NaN, y))
// 0
[NaN].includes(NaN)
// true
```
```javascript
// 拷贝对象及其原型属性，写法一的__proto__属性在非浏览器的环境不一定部署，因此推荐使用写法二和写法三
// 写法一
const clone1 = {
  __proto__: Object.getPrototypeOf(obj),
  ...obj
};

// 写法二
const clone2 = Object.assign(
  Object.create(Object.getPrototypeOf(obj)),
  obj
);

// 写法三
const clone3 = Object.create(
  Object.getPrototypeOf(obj),
  Object.getOwnPropertyDescriptors(obj)
)
```
```javascript
// 将查询字符串转为对象
Object.fromEntries(new URLSearchParams('foo=bar&baz=qux'))
// { foo: "bar", baz: "qux" }
```
```javascript
// 数组去重的又一方法
[...new Set(array)]
// 数组去重的又一方法
function dedupe(array) {
  return Array.from(new Set(array));
}
dedupe([1, 1, 2, 3]) // [1, 2, 3]
```
```javascript
// 字符串去重
[...new Set('ababbc')].join('')
// "abc"
```
```javascript
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

// 并集
let union = new Set([...a, ...b]);
// Set {1, 2, 3, 4}

// 交集
let intersect = new Set([...a].filter(x => b.has(x)));
// set {2, 3}

// 差集
let difference = new Set([...a].filter(x => !b.has(x)));
// Set {1}
```
```javascript
// map转为对象，如果所有 Map 的键都是字符串，它可以无损地转为对象，如果有非字符串的键名，那么这个键名会被转成字符串，再作为对象的键名
function strMapToObj(strMap) {
  let obj = Object.create(null);
  for (let [k,v] of strMap) {
    obj[k] = v;
  }
  return obj;
}
```
```javascript
// 对象转为map
function objToStrMap(obj) {
  let strMap = new Map();
  for (let k of Object.keys(obj)) {
    strMap.set(k, obj[k]);
  }
  return strMap;
}
```
```javascript
// map转为json，Map 的键名都是字符串，这时可以选择转为对象 JSON
function strMapToJson(strMap) {
  return JSON.stringify(strMapToObj(strMap));
}
// Map 的键名有非字符串，这时可以选择转为数组 JSON
function mapToArrayJson(map) {
  return JSON.stringify([...map]);
}
// JSON 转为 Map，正常情况下，所有键名都是字符串
function jsonToStrMap(jsonStr) {
  return objToStrMap(JSON.parse(jsonStr));
}

jsonToStrMap('{"yes": true, "no": false}')
// Map {'yes' => true, 'no' => false}
// 有一种特殊情况，整个 JSON 就是一个数组，且每个数组成员本身，又是一个有两个成员的数组。这时，它可以一一对应地转为 Map。这往往是 Map 转为数组 JSON 的逆操作
function jsonToMap(jsonStr) {
  return new Map(JSON.parse(jsonStr));
}

jsonToMap('[[true,7],[{"foo":3},["abc"]]]')
// Map {true => 7, Object {foo: 3} => ['abc']}
```
```javascript
// 利用 Generator 函数和for...of循环，实现斐波那契数列
function* fibonacci() {
  let [prev, curr] = [0, 1];
  for (;;) {
    yield curr;
    [prev, curr] = [curr, prev + curr];
  }
}

for (let n of fibonacci()) {
  if (n > 1000) break;
  console.log(n);
}
```
```javascript
// 利用for...of循环，可以写出遍历任意对象（object）的方法。原生的 JavaScript 对象没有遍历接口，无法使用for...of循环，通过 Generator 函数为它加上这个接口，就可以用了
function* objectEntries(obj) {
  let propKeys = Reflect.ownKeys(obj);

  for (let propKey of propKeys) {
    yield [propKey, obj[propKey]];
  }
}

let jane = { first: 'Jane', last: 'Doe' };

for (let [key, value] of objectEntries(jane)) {
  console.log(`${key}: ${value}`);
}
// first: Jane
// last: Doe

// 加上遍历器接口的另一种写法是，将 Generator 函数加到对象的Symbol.iterator属性上面
function* objectEntries() {
  let propKeys = Object.keys(this);

  for (let propKey of propKeys) {
    yield [propKey, this[propKey]];
  }
}

let jane = { first: 'Jane', last: 'Doe' };

jane[Symbol.iterator] = objectEntries;

for (let [key, value] of jane) {
  console.log(`${key}: ${value}`);
}
// first: Jane
// last: Doe
```
```javascript
```
```javascript
```
```javascript
```
```javascript
```
```javascript
```
```javascript
```
```javascript
```
```javascript
```
```javascript
```
### 前端项目
  + [基于vue elementui和springboot的微人事管理系统](https://github.com/lenve/vhr)

## html
## css
### css语法
### css预处理
### css技巧
  + [You-need-to-know-css](https://lhammer.cn/You-need-to-know-css/##/zh-cn/)
  + [QiShaoXuan-CSS Tricks](https://qishaoxuan.github.io/css_tricks/)
  + [CSS-Inspiration](https://chokcoco.github.io/CSS-Inspiration/##/)
## js框架
### vue
#### 插件
#### 组件
  + [element-ui](https://element.eleme.io/#/zh-CN)
#### 手脚架
  + [基于vue和element的后台管理](https://panjiachen.github.io/vue-element-admin-site/zh/guide/)
  + [vue-framework-wz后台管理](https://github.com/herozhou/vue-framework-wz)
  + []()
### react
### angular
## js库
## css框架
  + [bootstrap](https://foundation.zurb.com/)
  + [semantic ui](https://semantic-ui.com/)
  + [foundation](https://foundation.zurb.com/)
## css库
  + [animate动画库](https://daneden.github.io/animate.css/)
  + [iconfont](https://www.iconfont.cn/)
  + [Font Awesome图标库](https://fontawesome.com/)
  + [图表：echarts](https://echarts.baidu.com/echarts2/doc/example.html)
## 前端开发
### 基本开发流程
### pc端
### 移动端
## 工程化
### webpack
  + [webpack优化](https://juejin.im/post/5d372851f265da1ba915bf78)
  + [package.json说明文档](http://hao.caibaojian.com/t?url=https://juejin.im/post/5d3435b9f265da1b96134119)
### npm
## http
## 正则表达式
  + [匹配中国大陆手机号码](https://github.com/VincentSit/ChinaMobilePhoneNumberRegex)
## 网络资源
### 标准
  + [w3c](http://www.w3school.com.cn/)
  + [MDN](https://developer.mozilla.org/zh-CN/)
### 资料
  + [web开发路线图中文版](https://github.com/ccloli/developer-roadmap-zh-CN)
  + [git上的优质学习路线项目](https://github.com/wuxiaobin1995/Frontend-Doc)
  + [前端导航](https://yhlben.github.io/front-end-navigation/)
  + [菜鸟教程](https://www.runoob.com/)
  + [前端九部](https://www.yuque.com/fe9)
  + [css手册](http://css.doyoe.com/)
  + [阮一峰的个人网站](http://www.ruanyifeng.com/home.html)
  + [小火柴的前端小册子](https://xiaohuochai.site/)
  + [数据结构和算法](https://segmentfault.com/a/1190000019842169)
### 社区论坛
  + [github](https://github.com/)
  + [hellogitHub](https://hellogithub.com/)
  + [v2ex](https://www.v2ex.com/)
  + [掘金](https://juejin.im/timeline)
  + [思否](https://segmentfault.com/)
  + [开发者头条](https://toutiao.io/)
  + [infoQ](https://www.infoq.cn/)
### 期刊
  + [前端日报](http://caibaojian.com/t/%E5%89%8D%E7%AB%AF%E6%97%A5%E6%8A%A5)
  + [阿里云前端技术周刊](https://github.com/aliyunfe/weekly)
### 大厂前端博客
  + [淘宝前端团队](http://taobaofed.org/)
  + [语雀团队](https://txd.alibaba-inc.com/)
  + [百度云技术团队](https://fex.baidu.com/)
  + [百度用户体验中心](http://mux.baidu.com/)
  + [百度EFE](https://efe.baidu.com/)
  + [百度Eux](http://eux.baidu.com/)
  + [腾讯全段团队](http://www.alloyteam.com/)
  + [腾讯IMWeb前端团队](https://imweb.io/)
  + [腾讯用户研究与体验设计部](https://cdc.tencent.com/)
  + [360前端团队](https://75team.com/)
  + [去哪儿网大前端技术中心](https://ymfe.org/)
  + [京东设计中心](http://jdc.jd.com/)
  + [饿了么前端](https://zhuanlan.zhihu.com/ElemeFE)
  + [携程设计委员会](http://ued.ctrip.com/)
### 资源
  + [cdn加速服务：bootcdn](https://www.bootcdn.cn/)
  + [css样式重置：normalize](https://www.bootcdn.cn/normalize/)
### 面试
  + [InterviewMap](https://yuchengkai.cn/docs/)
### 工具
  + [占位图片生成器：dummyimage](https://dummyimage.com/)
  + [字体格式转换： fontsquirrel](https://www.fontsquirrel.com/tools/webfont-generator)
  + [兼容性检测：caniuse](https://caniuse.com/)
  + [本地服务构建工具：WampServer](http://www.wampserver.com/)
### 视频
  + 腾讯课堂
  + 慕课网
  + 51CTO
  + bilibili
### 刷题
  + [LeetCode](https://leetcode-cn.com/)
  + [动画的形式呈现解 LeetCode 题目的思路](https://github.com/MisterBooo/LeetCodeAnimation)
## node.js
## 设计模式
## 计算机原理
## 相关书籍
  + [git上分享的书](https://github.com/ddzy/fe-necessary-book)
## 优秀代码
