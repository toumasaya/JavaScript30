# 01 - JavaScript Drum Kit

[瀏覽成果](https://toumasaya.github.io/JavaScript30/01%20-%20JavaScript%20Drum%20Kit/index.html)

## 監聽事件

* 事件 `keydown`，當按下某個鍵可以發出聲音。查詢 keyCode：http://keycode.info/
* 事件 `transitionend`，偵測 transition

## DOM

* 透過 `data-*` 屬性可以方便的搜索物件
* `audio` 與 `video` 的 methods 與 properties 可以參考：https://www.w3schools.com/tags/ref_av_dom.asp
* 為元件增加 class：`element.classList.add(className)`
* 為元件移除 class：`element.classList.remove(className)`
* 找出 element：`document.querySelector(element)`
* 找出全部 element：`document.querySelectorAll(element)`

要注意的是 element 不是 Array，是 Node lists，所以無法使用 Array methods，例如 `addEventListener`。

但 Node lists 會有 `forEach` method 可以使用。

如果要把 Node lists 轉成 Array，可以使用 `Array.from`，例如：

```javascript
Array.from(document.querySelectorAll('.key'))
```
