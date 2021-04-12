# 业务文档

记录一些业务处理--Vue.js

## 后台管理系统

### 分页

element-ui 分页组件和 table 组件的分页做法：pagination 组件获取当前页码 curr 和 limits,slice 截取（ 当前页-1*limits, 当前页*limits )
[链接](https://segmentfault.com/a/1190000020087100)

## h5

### px 转换为rem

vue + px2rem [链接](https://blog.csdn.net/weixin_43607164/article/details/100512220)

## 判断是否过了一天的方法

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