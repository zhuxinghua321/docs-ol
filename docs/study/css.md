### 多行溢出
```css
  text-overflow: -o-ellipsis-lastline;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2; 
  line-clamp: 2;
  -webkit-box-orient: vertical;
```

### 单行
```css
  overflow: hidden;
  text-overflow:ellipsis;
  white-space: nowrap;
```

### flex布局，flex布局中剩下的一部分空间给有flex1的元素
```css
.flex1 {
          flex: 1 !important;
        }
```

### ::marker 文字前的符号
```css
li::marker {
  content: '';
}
可以配合:hover
li:hover::marker {
  content: '';
}
```