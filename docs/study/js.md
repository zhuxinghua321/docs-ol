### 判断多值匹配

```js
function test(type) {
  if (type === 'jpg' || type === 'png' || type === 'gif' || type === 'svg') {
    console.log("该文件为图片");
  }
}

function test(type) {
    const imgArr = ['jpg', 'png', 'gif', 'svg']
    if (imgArr.includes(type)) {
        console.log("该文件为图片")
    }
}

```

### 改造对象属性和属性值

```js
Array.map((v) => {
	return { name: v.item, id: v.id };
});
```

### 数组增删改查

<font style="color:#dd0000">indexOf</font> 用于查找数组中是否存在某个值，如果存在则返回某个值的下标，否则返回-1

<font style="color:#dd0000">includes</font> 检测数组中是否存在该元素，返回 Boolean 值

<font style="color:#dd0000">map</font> 一个数组函数方法，接收三个参数，value，index，self，返回值是处理完的结果。(forEach 类似，不过返回的是 undefined)

```js
let list = [1, 2, 3];

const res = list.map((value, key, self) => {
	console.log(value); // 1 2 3
	console.log(key); // 0 1 2
	console.log(self); // [1, 2, 3]
	return value * 2;
});
console.log(res);
```

还有 <font style="color:#dd0000">find,forEach,findindex</font>

<font style="color:#dd0000">splice</font> 用于数组删除或替换内容，接收三个参数：

第一个参数是，删除或添加的位置
第二个参数是，要删除的几位，如果为 0 则不删除
第三个参数是，向数组添加内容

```js
let list = [1, 2, 3];

list.splice(0, 1); // 把第0个位置，给删除一位
console.log(list); // [2, 3]

list.splice(0, 1, '蛙人'); // 把第0个位置，给删除一位，添加上一个字符串
console.log(list); // ["蛙人", 2, 3]

list.splice(0, 2, '蛙人'); // 把第0个位置，给删除2位，添加上一个字符串
console.log(list); // ["蛙人", 3]
```

<font style="color:#dd0000">slice</font> 用于截取数组值，接收两个参数，第一个参数是要获取哪个值的下标，第二个参数是截取到哪个下标的前一位。

<font style="color:#dd0000">filter</font> 用于过滤数组内的符合条件的值，返回值为满足条件的数组对象

<font style="color:#dd0000">push</font> 向数组后面添加元素，返回值为数组的 length

<font style="color:#dd0000">pop</font> 删除数组最后一个元素,返回值为删除的元素

<font style="color:#dd0000">shift</font> 删除第一个元素
<font style="color:#dd0000">unshift</font> 在数组头部添加一个元素

<font style="color:#dd0000">sort</font> 排序

### 数组技巧

数组方法 <font style="color:#dd0000">reduce</font>

```js
//求和
let nums = [1,22,31,4,56]
let sum = nums.reduce((prev, cur) => prev + cur,0)
```

数组去重 <font style="color:#dd0000">set</font> es6 利用 new Set 的集合有序列表，不能有重复

```js
let arr = [1, 1, 2, 434, 2, 1];
console.log([...new Set(arr)]); // 1 2 434

let arrNew = new Set();
arrNew.add(); //添加
arrNew.delete(); //删除
arrNew.clear(); //清空
```

数组与字符串的互相转换

```js
  let arr = [1,2,3,4];
  arr.join(',')  //返回 '1,2,3,4' 数组转为字符串以 , 拼接

  let str = '1,2,3,4';
  str.split(',') //返回 [1,2,3,4] 字符串转为数组以 , 分割
```

### 对象技巧

获取对象的 <font style="color:#dd0000">key -> ``Object.keys(obj)`` </font> 和 <font style="color:#dd0000">值 -> ``Object.values(obj)`` </font>

获取对象指定的值，es6 解构

```js
const person = { name: '蛙人', age: 24, sex: 'male' };
let { age, sex } = person;
console.log(age, sex); // 24 male
```

<font style="color:#dd0000">``Object.assign``</font> 对象合并

```js
let person = { name: '蛙人', age: 24 };
let obj = Object.assign({}, person);
console.log(obj); // {name: "蛙人", age: 24}
```

### 运算符

<font style="color:#dd0000">??</font> 运算符只有前面的值是 undefined or null 才会执行

```js
let status = undefined;
let text = status ?? '暂无';
console.log(text); // 暂无
```

<font style="color:#dd0000">~ ~</font>  双非运算符可以用于向下取整。 console.log(~~4.3) // 4

### 判断是否为空

```js
 if(str != null && typeof(str) != undefined && str !=''){
    console.log(str + ' 不为空');
 }
```

### 判断值类型
```js
  function type(params) {
      return Object.prototype.toString.call(params);
    } 
```

### 获取滚动条的滚动距离
```js
function getScrollOffset() {
    if (window.pageXOffset) {
        return {
            x: window.pageXOffset,
            y: window.pageYOffset
        }
    } else {
        return {
            x: document.body.scrollLeft + document.documentElement.scrollLeft,
            y: document.body.scrollTop + document.documentElement.scrollTop
        }
    }
}
```
### 获取浏览器地址参数
```js
function getWindonHrefParams() {
      var sHref = window.location.href;
      var args = sHref.split('?');
      if (args[0] === sHref) {
        return '';
      }
      var hrefarr = args[1].split('#')[0].split('&');
      var obj = {};
      for (var i = 0; i < hrefarr.length; i++) {
        hrefarr[i] = hrefarr[i].split('=');
        obj[hrefarr[i][0]] = hrefarr[i][1];
      }
      return obj;
    }
```