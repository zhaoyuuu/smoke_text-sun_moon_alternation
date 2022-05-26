# 文字烟雾效果
## 大致（关键）步骤
1. 文字水平垂直居中
2. 正则匹配每个字符替换成\<span>包裹的字符节点（**重点**）
3. 给每个字符节点添加鼠标事件（mouseover时给字符节点添加active类名）
4. active类的样式是一个animation：（forwards是让动画停在最后的画面）
`animation: smoke 2s linear forwards;` 
5. smoke动画：
- `filter：blur()`   实现毛玻璃的效果
- transform: rotate()  实现文字像烟雾一样的上升旋转效果
```css
@keyframes smoke {
  0% {
    opacity: 1;
    filter: blur(0);
    transform: translateX(0) translateY(0)  scale(1) rotate(0)
  }
  100% {
    opacity: 0;
    filter: blur(20px);
    transform: translateX(200px) translateY(-300px) scale(4) rotate(720deg)
  }
}
```
## 实践中注意的小点：
- translate需要元素是block / inline-block
- tranlateX()和translate(x,y)
```css
// translateX / Y
transform: translateX(200px);
// translate()
transform: translate(200px, 200px);
```
- JS判断节点是否有某个类名：`el.classList.contains('类名')` 
- JS给节点添加类名：`el.classList.add('类名')` 
<br/>
<br/>
<br/>
<br/>

# 日月交替效果
## 大致（关键步骤）
1. moon和sun水平垂直居中（position定位）
2. 利用box-shadow巧妙画出月亮：
```css
// .moon是圆的，偏移一下就是月亮形状
box-shadow: 160px 180px 0 cyan;  
// translate将月亮平移至中央
transform: translate(calc(-50% + -160px), calc(-50% + -180px));  // 注意这里要计算一下偏移位置
```
3. “大海”效果关键代码：`backdrop-filter: blur(100px);` 
> backdrop-filter 为一个元素后面区域添加图形效果（如模糊或颜色偏移）。
4. 给各节点添加动画
```css
.sun,
.moon,
.sun-box,
.moon-box,
.bg {
  transition: all 1s ease;
}
```
5. 白天.light   /   夜晚.dark，切换的时候给大容器分别赋予两个类名，实现日月交替和背景变化

## 实践中注意的小点：
- 利用z-index给图层排序
- 想要切换的时候月亮或太阳消失，需要利用其相对盒子的定位，并给盒子添加溢出隐藏
```css
// 给月亮/太阳盒子添加相对定位
.moon-box, 
.sun-box {
  position: relative;  // moon/sun相对其盒子定位
  overflow: hidden;  // 溢出隐藏
}
```