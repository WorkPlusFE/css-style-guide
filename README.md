# WorkPlusFE CSS 编写指南

当前版本 v0.1.0

> 用更合理、更优雅、更高效的方式编写 CSS

## 目录

[术语](#术语)
  * [规则声明](#规则声明)
  * [选择器](#选择器)
  * [属性](#属性)

[CSS](#CSS)
  * [格式](#格式)
  * [注释](#注释)
  * [OOCSS-和-BEM](#OOCSS-和-BEM)
  * [ID-选择器](#ID-选择器)
  * [JavaScript-钩子](#JavaScript-钩子)
  * [边框](#边框)

## 术语

### 规则声明

我们把一个（或一组）选择器和一组属性称之为 “规则声明”。举个例子：

```css
.listing {
  font-size: 18px;
  line-height: 1.2;
}
```

### 选择器

在规则声明中，“选择器” 负责选取 DOM 树中的元素，这些元素将被定义的属性所修饰。选择器可以匹配 HTML 元素，也可以匹配一个元素的类名、ID, 或者元素拥有的属性。以下是选择器的例子：

```css
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

### 属性

最后，属性决定了规则声明里被选择的元素将得到何种样式。属性以键值对形式存在，一个规则声明可以包含一或多个属性定义。以下是属性定义的例子：

```css
/* some selector */ {
  background: #f1f1f1;
  color: #333;
}
```

## CSS

### 格式

* 使用 2 个空格作为缩进；
* 类名字母必须为小写，优先使用 BEM 方式进行类的命名；（[关于 OOCSS 和 BEM](#oocss-and-bem)）
* 不要使用 ID 选择器；
* 在一个规则声明中应用了多个选择器时，每个选择器独占一行；
* 在规则声明的左大括号 `{` 前加上一个空格；
* 在属性的冒号 `:` 后面加上一个空格，前面不加空格；
* 规则声明的右大括号 `}` 独占一行；
* 规则声明之间用空行分隔开。

**Bad**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**Good**

```css
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### 注释

* 建议使用行注释代替块注释；
* 建议注释独占一行，避免行末注释；
* 给没有自注释的代码写上详细说明，比如：
  - 为什么用到了 z-index
  - 兼容性处理或者针对特定浏览器的 hack

### OOCSS 和 BEM

出于以下原因，我们鼓励使用 OOCSS 和 BEM 的某种组合：

  * 可以帮助我们理清 CSS 和 HTML 之间清晰且严谨的关系。
  * 可以帮助我们创建出可重用、易装配的组件。
  * 可以减少嵌套，降低特定性。
  * 可以帮助我们创建出可扩展的样式表。

**OOCSS**，也就是 “Object Oriented CSS（面向对象的CSS）”，是一种写 CSS 的方法，其思想就是鼓励你把样式表看作“对象”的集合：创建可重用性、可重复性的代码段让你可以在整个网站中多次使用。

参考资料：

  * Nicole Sullivan 的 [OOCSS wiki](https://github.com/stubbornella/oocss/wiki)
  * Smashing Magazine 的 [Introduction to OOCSS](http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/)

**BEM**，也就是 “Block-Element-Modifier”，是一种用于 HTML 和 CSS 类名的_命名约定_。BEM 最初是由 Yandex 提出的，要知道他们拥有巨大的代码库和可伸缩性，BEM 就是为此而生的，并且可以作为一套遵循 OOCSS 的参考指导规范。

  * CSS Trick 的 [BEM 101](https://css-tricks.com/bem-101/)
  * Harry Roberts 的 [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

**示例**

```html
<article class="listing-card listing-card--featured">

  <h1 class="listing-card__title">Adorable 2BR in the sunny Mission</h1>

  <div class="listing-card__content">
    <p>Vestibulum id ligula porta felis euismod semper.</p>
  </div>

</article>
```

```css
.listing-card { }
.listing-card--featured { }
.listing-card__title { }
.listing-card__content { }
```

  * `.listing-card` 是一个块（block），表示高层次的组件。
  * `.listing-card__title` 是一个元素（element），它属于 `.listing-card` 的一部分，因此块是由元素组成的。
  * `.listing-card--featured` 是一个修饰符（modifier），表示这个块与 `.listing-card` 有着不同的状态或者变化。

### ID 选择器

在 CSS 中，虽然可以通过 ID 选择元素，但大家通常都会把这种方式列为反面教材。ID 选择器给你的规则声明带来了不必要的高[优先级](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity)，而且 ID 选择器是不可重用的。

想要了解关于这个主题的更多内容，参见 [CSS Wizardry 的文章](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/)，文章中有关于如何处理优先级的内容。

### JavaScript 钩子

避免在 CSS 和 JavaScript 中绑定相同的类。否则开发者在重构时通常会出现以下情况：轻则浪费时间在对照查找每个要改变的类，重则因为害怕破坏功能而不敢作出更改。

我们推荐在创建用于特定 JavaScript 的类名时，添加 `.js-` 前缀：

```html
<button class="btn btn-primary js-request-to-book">Request to Book</button>
```

### 边框

在定义无边框样式时，使用 `0` 代替 `none`。

**Bad**

```css
.foo {
  border: none;
}
```

**Good**

```css
.foo {
  border: 0;
}
```
