# 业务文档

记录一些业务处理

## 后台管理系统

### 分页

element-ui 分页组件和 table 组件的分页做法：pagination 组件获取当前页码 curr 和 limits,slice 截取（ 当前页-1*limits, 当前页*limits )
[链接](https://segmentfault.com/a/1190000020087100)

## h5

### px 自适应，全局引用

```js
const baseSize = 32;

function setRem() {
	const scale = document.documentElement.clientWidth / 750;
	document.documentElement.style.fontSize =
		baseSize * Math.min(scale, 2) + 'px';
}

setRem();

window.onresize = setRem;
```
