# 业务文档
记录一些业务处理--Vue.js
### 分页
element-ui 分页组件和 table 组件的分页做法：pagination 组件获取当前页码 curr 和 limits,slice 截取（ 当前页-1*limits, 当前页*limits )
[链接](https://segmentfault.com/a/1190000020087100)
### px 转换为rem
vue + px2rem [链接](https://blog.csdn.net/weixin_43607164/article/details/100512220)
### 判断是否过了一天的方法
```js
handleAfterDay () {
      let today = new Date().setHours(0, 0, 0, 0);//当天时间00:00:00
      let oldTime = localStorage.getItem('date');
      if(!oldTime){
        localStorage.setItem('showTime', today); //没有记录的时候，记录当天时间
        ...  
      }else if (today != oldTime) {
        //现在时间  记录的时间
        localStorage.setItem('showTime', today); //记录当天
        ...
      };
    },
```
### 定时器，轮询机制
```js
   index: 0, //记录定时器次数
   timer: null, //定时器

  //  3秒一次，执行10次
  checkStatus () {
      if (this.index < 10) {
        if (this.index === 0) {
          // 立即执行一次
          this.timer = setInterval(() => { this.checkStatus() }, 3000) //开启
        }
        this.index++;
      } else {
        clearInterval(this.timer); //清除定时器
        this.index = 0; 
      }
    },
```
### 生成随机数字
```js
/**
   * @desc 生成随机数字
   * @func
   * @param {number} [max]=1 - 最大值，默认值为1
   * @param {number} [min]=0 - 最小值，默认值为0
   * @param {number} [fixed]=0 - 小数点位数，默认值为0
   * @return {string}
   * */
  rNumber(max = 1, min = 0, fixed = 0) {
    max < min && (max = [min, min = max][0]);
    return (Math.random() * (max - min) + min).toFixed(fixed);
  }
```
### 生成随机字符串
```js
/**
   * @desc 生成随机字符串
   * @func
   * @param {number} [len]=1 - 字符串长度，默认值为1
   * @param {string} [type]='a' - 生成字符串包含字符类型（数字，字母（区分大小写），汉字），默认值为a
   * @param {string} [otherStr]='' - 除上术类型中字符类型外包含的字符
   * @param {boolean} [canRepeat]=true - 字符是否可以重复， 默认可以重复
   * @return {string}
   * @example
   * rString(3, '0a', '*') //生成的字符串长度为3，可能包含数字（type中有数字），小写字母（type中有小写字母）和‘*’号
   * */
  rString(len, type, otherStr, canRepeat = true) {
    typeof type !== 'string' && (type = 'a');
    if (typeof otherStr === 'string') {
      otherStr = otherStr.split('');
    } else if (Array.isArray(otherStr)) {
      otherStr = otherStr.filter(e => e && typeof e === 'string');
    } else {
      otherStr = [];
    }
    otherStr = otherStr.map(e => [e.charCodeAt(), e.charCodeAt()]);
    isNaN(len) && (len = 1);
    let str = [];
    let reg = {
      '[a-z]': [97, 122], // a-z
      '[A-Z]': [65, 90], // A-Z
      '[0-9]': [48, 57], // 0-9
      '[\u4e00-\u9fa5]': [19968, 40869], // 汉字
    };
    let regs = Object.keys(reg)
      .filter(k => new RegExp(k).test(type))
      .map(e => reg[e]);
    regs.push(...otherStr);
    for (let l = regs.length; len > 0 && l; len--) {
      let m = this.rNumber(l - 1, 0) | 0;
      let s = regs[m];
      let n = this.rNumber(s[1], s[0]) | 0;
      if (!canRepeat && (str.includes(s) || str.includes(String.fromCharCode(n)))) {
        len++;
        if (typeof s === 'string') {
          regs.splice(m, 1);
          l--;
        }
        continue;
      }
      str.push(typeof s === 'string' ? s : String.fromCharCode(n));
    }
    return str.filter(e => e).join('');
  }

```
### 10位时间戳转化为年Y 月M 日D 时H 分m 秒s 毫秒S
```js
/**
   * @desc 10位时间戳转化为年Y 月M 日D 时H 分m 秒s 毫秒S
   * @function
   * @params {number/date} number: 传入10位时间戳 / Date对象
   * @params {string} [format]：返回格式
   * @return {string}
   */
  formatDate(number, format = 'Y-MM-DD HH:mm:ss') {
    let time = number instanceof Date ? number : !isNaN(number) && number !== '' ? new Date(number * 1000) : false;
    if (time === false) return '';
    let obj = {
      'Y': time.getFullYear(),
      'M': time.getMonth() + 1,
      'W': ['七', '一', '二', '三', '四', '五', '六'][time.getDay()],
      'w': time.getDay() || 7,
      'D': time.getDate(),
      'A': time.getHours() > 12 ? 'PM' : 'AM',
      'a': time.getHours() > 12 ? 'pm' : 'am',
      'H': time.getHours(),
      'h': time.getHours() % 12 || 12,
      'm': time.getMinutes(),
      's': time.getSeconds(),
      'S': time.getTime() % 1000,
    };
    format = format.replace(/([YMWDAHSwahms])\1*/g, e => e.length === 2 && !/[SaAWw]/g.test(e) ? ('00' + obj[e[0]]).slice(-2) : obj[e[0]]);
    return format;
  }
```
### 毫米数转时间
```js
/**
   * @desc 毫秒数转时间 (日D 时H 分m 秒s 毫秒S)
   * @param {number} number 要转换的毫秒数
   * @param {string} [format] 转换的格式
   * @return String
   * */
  numberToTime(number, format = 'HH:mm:ss') {
    number = isNaN(number) ? 0 : (number || 0);
    let regs = {
      S: number | 0,
      s: number / 1000 | 0,
      m: number / 1000 / 60 | 0,
      H: number / 1000 / 60 / 60 | 0,
      D: number / 1000 / 60 / 60 / 24 | 0,
    };
    // 保留要输出的最高位 如：防止 format = 'mm:ss' 时 '60:00' 变为 '00:00'
    if (format.indexOf('s') >= 0) regs.S %= 1000;
    if (format.indexOf('m') >= 0) regs.s %= 60;
    if (format.indexOf('H') >= 0) regs.m %= 60;
    if (format.indexOf('D') >= 0) regs.H %= 24;

    format = format.replace(/([smhd])\1*/ig, e => {
      let val = regs[e[0]] || 0;
      val += '';
      if (e.length === 2 && val.length < 2) {
        return e.indexOf('S') < 0 ? '0' + val : val.slice(0, 2);
      } else {
        return val;
      }
    });
    return format;
  }
```
### 返回相对于当前时间的时间段
```js
/**
   * @desc 返回当前日期的相对时间段
   * @function
   * @params {number/string} val:
   *         类型 n(近n天) , tm(本月), ty(本年), nlm(前n月), nly(前n年), nm(近n月), ny(近n年)
   * @return {array} [start(00:00:00), end(23:59:59)]
   */
  typeToDate(val) {
    if (typeof val !== 'number' && typeof val !== 'string') return [,];
    let start = new Date();
    let end = new Date();
    let num = parseInt(val);
    if (!isNaN(val)) {
      // 近num天 num
      val && start.setDate(start.getDate() - num);
    } else if (/tm$/i.test(val)) {
      // 本月 tm
      start.setDate(1);
      end.setMonth(end.getMonth() + 1);
      end.setDate(0);
    } else if (/ty$/i.test(val)) {
      // 本年 ty
      start.setMonth(0);
      start.setDate(1);
      end.setMonth(11);
      end.setDate(31);
    } else if (/l$/i.test(val)) {
      // 前num天 nl
      end.setDate(end.getDate() - 1);
      start.setDate(start.getDate() - num);
    } else if (/lm$/i.test(val)) {
      // 前num个月 nlm
      end.setDate(0);
      start.setMonth(start.getMonth() - num);
      start.setDate(1);
    } else if (/ly$/i.test(val)) {
      // 前num年 nly
      end.setMonth(-1);
      end.setDate(31);
      start.setFullYear(start.getFullYear() - num);
      start.setMonth(0);
      start.setDate(1);
    } else if (/m$/i.test(val)) {
      // 近num月 nm
      start.setMonth(start.getMonth() - num);
    } else if (/y$/i.test(val)) {
      // 近num年 ny
      start.setFullYear(start.getFullYear() - num);
    }
    let arr = [start, end];
    arr = arr.map((d, i) => new Date(this.formatDate(d, 'Y-M-D ' + (!i ? '00:00:00' : '23:59:59'))));
    return arr;
  }
```
### 复制文本
```js
/**
   * 复制文本到剪切板
   * @param {String | number} text 要复制的文本
   * @return Boolean
   * */
  copyText(text) {
    let input = document.createElement('input');
    input.style.position = 'fixed';
    input.style.top = '-1000px';
    document.body.appendChild(input);
    input.value = text;
    input.select();
    let res = document.execCommand('copy');
    document.body.removeChild(input);
    return res;
  }
```
### 获取url地址的参数
```js
/**
   * 获取地址上 /？。。。#/ 间的数据
   * @param [key] 获取此字段的数据 不传返回包含所有数据的对象
   * */
  getQuery(key) {
    let query = {};
    location.search.slice(1).split('&').map(str => {
      let arr = str.split('=');
      arr[0] && (query[arr[0]] = arr[1]);
    });
    return key ? query[key] : query;
  }
```