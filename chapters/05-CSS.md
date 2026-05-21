# 五、CSS 面试八股（含案例讲解）

[← 返回目录](../README.md)

---

## 1. 盒子模型

```css
.box {
  box-sizing: border-box; /* 宽高含 padding+border，布局更好算 */
  width: 200px;
  padding: 10px;
  border: 2px solid #ccc;
  margin: 16px;
}
/* content-box（默认）：width 只算内容区 */
```

---

## 2. BFC：解决 margin 塌陷 & 清除浮动

```html
<div class="parent">
  <div class="child"></div>
</div>
```

```css
.child { margin-top: 20px; }
/* 子 margin 可能「塌」到父元素外 */

.parent {
  overflow: hidden; /* 触发 BFC，包住子 margin */
}
/* 或 display: flow-root; 专门用来创建 BFC */
```

**清除浮动**：父级 `overflow: hidden` 或伪元素 `.clearfix::after { content:''; display:block; clear:both; }`

---

## 3. 垂直水平居中（Flex 最常用）

```css
.center-wrap {
  display: flex;
  justify-content: center; /* 主轴水平 */
  align-items: center;     /* 交叉轴垂直 */
  min-height: 100vh;
}
```

---

## 4. CSS 权重

```css
/* 权重：!important > 行内 > #id > .class > 标签 */
#app .btn { color: blue; }   /* 0,1,1,0 */
.btn { color: red; }         /* 被上面覆盖 */
```

**案例**：样式不生效 → 打开 DevTools 看被哪条规则覆盖、是否权重不够。

---

[下一章：Vue2 →](./06-Vue2.md)
