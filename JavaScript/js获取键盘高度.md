## 获取键盘弹出前窗口可视区域大小 ##
```JavaScript
const beforeHeight = document.documentElement.clientHeight || document.body.offsetHeight;
```

## 获取键盘弹出后可视区域大小 ##
```JavaScript
const afterHeight = document.documentElement.clientHeight || document.body.offsetHeight;
```

## 两者相互减就是：键盘高度 ##
```JavaScript
const keyboardHeight = beforeHeight - afterHeight;
```

注意： 不要在键盘弹出后，立即获取键盘弹出后的可视区域大小，因为此时可视区域还没有初始化完成，获取到的结果还是键盘弹出前的值，最好设置setTimeout为600，之后获取可视区域的高度。
