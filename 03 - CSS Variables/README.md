# 03 - CSS Variables

瀏覽成果

## CSS Variable 的用法

CSS Variable 可以直接在 CSS 寫變數，目前 IE 與 Edge 尚未支援。

寫起來會像是這樣：

```css
:root {
  --main-color: #000;
}

h1 {
  color: var(--main-color);
}
```

定義變數的時候前面要加 `--`，使用時要用 `var(variable_name)`。

更多資料可以參考：

* [Using CSS variables](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_variables)
* [CSS Variables: Why Should You Care?](https://developers.google.com/web/updates/2016/02/css-variables-why-should-you-care)
* [CSS Custom Properties for Cascading Variables Module Level 1](https://www.w3.org/TR/css-variables/#using-variables)
* [:root](https://developer.mozilla.org/zh-TW/docs/Web/CSS/:root)

## document.documentElement

參考以下：

```javascript
const x = document.documentElement.nodeName;

// return: HTML
```

所以 `document.documentElement` 是代表 `<html>` element 物件。

然後監聽所有的 inputs 的 `change` `mousemove` 事件，並且加上一個 function `handleUpdate`：

```javascript
inputs.forEach( input => input.addEventListener('change', handleUpdate));
inputs.forEach( input => input.addEventListener('mousemove', handleUpdate));
```

在 `handleUpdate` function 要做的事情就是幫 `<html>` element 加上 `style`，還要加上後綴單位：

```javascript
function handleUpdate() {
  const suffix = this.dataset.sizing || '';
  document.documentElement.style.setProperty(`--${this.name}`, this.value + suffix);
}
```

因為 `--base` 顏色沒有單位，所以加上 `|| ''` 判斷如果沒有 `data-sizing` 就使用空字串。

更多 DOM 資訊可以參考：

* [HTML DOM documentElement Property](https://www.w3schools.com/jsref/prop_document_documentelement.asp)
* [The HTML DOM Element Object](https://www.w3schools.com/jsref/dom_obj_all.asp)
* [HTML DOM Style Object](https://www.w3schools.com/jsref/dom_obj_style.asp)
