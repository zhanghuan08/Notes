# CSS基础

##选择器（Selectors）

**元素选择器**

> ​		通过元素名称，设置元素属性。

例：

```css
div {
    color: #fff
}
```



**类选择器（Class selectors）**

>​		通过设置元素的 [`class`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes#attr-class) 属性，可以为元素指定类名。类名由开发者自己指定。 文档中的多个元素可以拥有同一个类名。

例：

```css
.key {
    color: #fff
}
```

**ID选择器（ID selectors）**

>​		通过设置元素的 [`id`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes#id) 属性为该元素制定ID。ID名由开发者指定。每个ID在文档中必须是唯一的。

```css
#principal {
  font-weight: bolder;
}
```

> 你也可以将多个选择器组合起来构成更确定的选择器。
>
> 比如，选择器`.key` 选中所有class属性为 `key的元素`. 选择器 `p.key` 选中所有class属性为key的`<p>`元素。
>
> 除了`class` 和 `id，你还可以用方括号的形式指定其他属性。比如`，
>
> 选择器 `[type='button']` 选中所有 `type` 属性为 `button 的元素。`

**伪类选择器（Pseudo-classes selectors）**

>​		CSS伪类（[pseudo-class](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Pseudo-classes)）是加在选择器后面的用来指定元素状态的关键字。比如，[`:hover`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:hover) 会在鼠标悬停在选中元素上时应用相应的样式。

```css
.a:hover {
     color: #fff
}
```

#### 伪类列表

- [`:link`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:link)
- [`:visited`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:visited)
- [`:active`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:active)
- [`:hover`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:hover)
- [`:focus`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:focus)
- [`:first-child`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:first-child)
- [`:nth-child`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:nth-child)
- [`:nth-last-child`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:nth-last-child)
- [`:nth-of-type`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:nth-of-type)
- [`:first-of-type`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:first-of-type)
- [`:last-of-type`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:last-of-type)
- [`:empty`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:empty)
- [`:target`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:target)
- [`:checked`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:checked)
- [`:enabled`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:enabled)
- [`:disabled`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:disabled)