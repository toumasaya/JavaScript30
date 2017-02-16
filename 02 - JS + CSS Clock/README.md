# 02 - JS + CSS Clock

[瀏覽成果](https://toumasaya.github.io/JavaScript30/02%20-%20JS%20%2B%20CSS%20Clock/index.html)

## Date()

使用 `new Date()` 來產生日期時間，更多 `Date()` methods 可以參考：https://www.w3schools.com/js/js_date_methods.asp

本練習中使用 `getSeconds()` `getMinutes()` `getHours()` 來獲取時、分、秒：

```javascript
const now = new Date();
const seconds = now.getSeconds();
const minutes = now.getMinutes();
const hours = now.getHours();
```

`const now = new Date()` 會建立一個新的 `Date` object，如果要轉成其他格式例如 String，就要再加上：

```javascript
now.toString()
```

更多資訊可以參考：

* [Date.parse()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/parse)

## 度數轉換

取得時間之後，要把時間轉換成度數，這樣才能模擬指針轉動的效果。

Wes 提供的解法是：

```javascript
const now = new Date();
const seconds = now.getSeconds();
const minutes = now.getMinutes();
const hours = now.getHours();
const secondsDegree = (seconds / 60) * 360 + 90;
const minutesDegree = (minutes / 60) * 360 + 90;
const hoursDegree = ((hours / 12) * 360) + 90;
```

加上 `90` 是因為一開始在 CSS 的設定上把指針都轉了 90 度，所以要加上 `90` 才會正確轉到想要的時間。

但如果想要模擬更逼真的指針效果，例如讓時針在一小時內緩慢的移動到下一小時，可以加上分鐘，去計算每一分鐘對時針的角度影響，同理，分針也可以加上秒鐘的角度。

```javascript
const secondsDegree = (seconds / 60) * 360 + 90;
const minutesDegree = ((minutes / 60) * 360 + 90) + (seconds / 60 / 60) * 360;
const hoursDegree = (((hours / 12) * 360) + 90) + ((minutes / 60 / 12) * 360) + (seconds / 60 / 60 / 12) * 360;
```

最後再把轉換完的度數，利用 `style` 把樣式加在指針的元件上：

```javascript
secondHand.style.transform = `rotate(${secondsDegree}deg)`
minHand.style.transform = `rotate(${minutesDegree}deg)`
hourHand.style.transform = `rotate(${hoursDegree}deg)`
```

## 小問題

在秒針進行轉動時，過了一圈之後，開始第二圈時，角度會變成：444° -> 90° -> 96°，由於加了 `transition` 屬性，過渡期間只有 0.05s 所以當下看就會閃一下，Wes 提示要解決有兩種方式，一種是當秒針回到終點要繼續下一圈時，把 `transition` 的時間設為 0，第二種是讓角度持續增加。

第一種解法：

```javascript
if (secondsDegree === 90) secondHand.style.transition = 'all 0s';
else secondHand.style.transition = 'all 0.05s';
```

第二種解法，就是先定義一個初始化的時間 `initDate()`，然後再定義一個 `updateDate()`，一開始先執行一次 `initDate`，接著再使用 `updateDate`。

[第二種解法成果](https://toumasaya.github.io/JavaScript30/02%20-%20JS%20%2B%20CSS%20Clock/index-advance.html)

```javascript
let secondsDegree = 0,
    minutesDegree = 0,
    hoursDegree = 0;

function initDate() {
  const now = new Date();
  const seconds = now.getSeconds();
  const minutes = now.getMinutes();
  const hours = now.getHours();
  secondsDegree = (seconds / 60) * 360 + 90;
  minutesDegree = ((minutes / 60) * 360 + 90) + (seconds / 60 / 60) * 360;
  hoursDegree = (((hours / 12) * 360) + 90) + ((minutes / 60 / 12) * 360) + (seconds / 60 / 60 / 12) * 360;
}

function updateDate() {
  secondsDegree += (1 / 60) * 360;
  minutesDegree += (1 / 60 / 60) * 360;
  hoursDegree += (1 / 60 / 60 / 12) * 360;

  secondHand.style.transform = `rotate(${secondsDegree}deg)`;
  minHand.style.transform = `rotate(${minutesDegree}deg)`;
  hourHand.style.transform = `rotate(${hoursDegree}deg)`;
}

initDate();
setInterval(updateDate, 1000)
```

可以參考：[Soyaine 写的中文指南](https://github.com/soyaine/JavaScript30)

## images placeholder

[Unsplash It](https://unsplash.it/) 可以讓你透過連結快速製作出假圖。
