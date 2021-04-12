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