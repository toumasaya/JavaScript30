# 04 - Array Cardio Day 1

這個練習介紹了常用的 Array methods：

```javascript
Array.prototype.filter() // ES5
Array.prototype.map()    // ES5
Array.prototype.reduce() // ES5
Array.prototype.sort()
```

## ECMAScript 5 陣列方法

* method 的第一個 argument 大多是個函式
* 函式會用三個 arguments 來調用：
  * 陣列元素值（the value of the array element）
  * 陣列元素的索引（the index of the array element）
  * 陣列本身（the array itself）

通常只需要第一個 argument。

所有的 ECMAScript 5 陣列方法都不會修改調用它們的陣列，會回傳一個新陣列。

參考來源：JavaScript 大全（第六版）

## Array.prototype.filter()

語法：

```javascript
array.filter(function(currentValue, index, arr), thisValue)
```

`Array.prototype.filter()` 會對陣列的每個元素，調用給定的 callback 函式，並且建立一個能讓 callback 回傳 `true` 或 truthy 元素的新陣列。

篩選 16 世紀出生的發明家：

```javascript
const inventors = [
  { first: 'Albert', last: 'Einstein', year: 1879, passed: 1955 },
  { first: 'Isaac', last: 'Newton', year: 1643, passed: 1727 },
  { first: 'Galileo', last: 'Galilei', year: 1564, passed: 1642 },
  { first: 'Marie', last: 'Curie', year: 1867, passed: 1934 },
  { first: 'Johannes', last: 'Kepler', year: 1571, passed: 1630 },
  { first: 'Nicolaus', last: 'Copernicus', year: 1473, passed: 1543 },
  { first: 'Max', last: 'Planck', year: 1858, passed: 1947 }
];

const fifteen = inventors.filter(function(inventor) {
  if (inventor.year >= 1500 && inventor.year < 1600) {
    return inventor;
  }
});
console.table(fifteen); // 可以用表格方式排列資料
```

由於 `filter()` method 只會保留 `true` 的元素，搭配 Arrow function 可以寫得更精簡：

```javascript
const fifteen = inventors.filter(inventor => inventor.year >= 1500 && inventor.year < 1600)
```

參考資料：[MDN - Array.prototype.filter()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)

## Array.prototype.map()

語法：

```javascript
array.map(function(currentValue, index, arr), thisValue)
```

`Array.prototype.map()` 會對陣列的每個元素，調用給定的 callback 函式，並且回傳一個新陣列。

展現發明家的名與姓：

```javascript
const inventors = [
  { first: 'Albert', last: 'Einstein', year: 1879, passed: 1955 },
  { first: 'Isaac', last: 'Newton', year: 1643, passed: 1727 },
  { first: 'Galileo', last: 'Galilei', year: 1564, passed: 1642 },
  { first: 'Marie', last: 'Curie', year: 1867, passed: 1934 },
  { first: 'Johannes', last: 'Kepler', year: 1571, passed: 1630 },
  { first: 'Nicolaus', last: 'Copernicus', year: 1473, passed: 1543 },
  { first: 'Max', last: 'Planck', year: 1858, passed: 1947 }
];

const fullNames = inventors.map(inventor => `${inventor.first} ${inventor.last}`);
```

參考資料：[MDN - Array.prototype.map()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

## Array.prototype.reduce()

語法：

```javascript
array.reduce(function(total, currentValue, currentIndex, arr), initialValue)
```

`reduce()` 簡單來說就是會把陣列元素值結合或化簡為一個值。

舉例，如果要算出陣列元素值的總和：

```javascript
const array = [1,2,3,4,5];

const sum = array.reduce((total, currentValue) => {
  return total + currentValue
}, 0);

console.log(sum); // 15
```

`reduce()` 取兩個 arguments，第一個是執行 reduction 的函式，第二個是要傳給函式的初始值。

以上數加總為例，在傳給 `reduce()` 的函式中有兩個 arguments，`total` 與 `currentValue`，`total` 是目前累積結果，第一次調用函式時，`total` 的值會是你傳入的初始值 `0`。

在後續調用中，`total` 就會是前一次調用時回傳的值：

1. 第一次調用函式，會傳入初始值 `0`，`total` 此時就是 `0`，`currentValue` 是 `1`，然後執行 `0 + 1`，回傳 `1`
2. 接著 `total` 就變成 `1`, `currentValue` 變成 `2`，執行 `1 + 2`，回傳 `3`
3. 如此繼續計算，`3 + 3 = 6`...`6 + 4 = 10`...`10 + 5 = 15`，最後就是回傳 `15`

練習的例子是，算出發明家的歲數總和，由於資料只提供出生年份和逝世年份，所以用 `passed - year` 就可以算出活多久歲數，再把這些歲數加總起來：

```javascript
const inventors = [
  { first: 'Albert', last: 'Einstein', year: 1879, passed: 1955 },
  { first: 'Isaac', last: 'Newton', year: 1643, passed: 1727 },
  { first: 'Galileo', last: 'Galilei', year: 1564, passed: 1642 },
  { first: 'Marie', last: 'Curie', year: 1867, passed: 1934 },
  { first: 'Johannes', last: 'Kepler', year: 1571, passed: 1630 },
  { first: 'Nicolaus', last: 'Copernicus', year: 1473, passed: 1543 },
  { first: 'Max', last: 'Planck', year: 1858, passed: 1947 }
];

const totalYears = inventors.reduce((total, inventor) => {
  return total + (inventor.passed - inventor.year);
}, 0);
```

除此之外，`reduce()` 還可以這樣使用：

```javascript
// 統計陣列元素項目的數量，回傳一個 object
const data = ['car', 'car', 'truck', 'truck', 'bike', 'walk', 'car', 'van', 'bike', 'walk', 'car', 'van', 'car', 'truck' ];

const transportation = data.reduce((obj, item) => {
  if(!obj[item]) {
    obj[item] = 0;
  }
  obj[item]++
  return obj;
}, {});
console.log(transportation);
```

參考資料：

* JavaScript 大全（第六版）
* [MDN - Array.prototype.reduce()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

## Array.prototype.sort()

語法：

```javascript
array.sort(compareFunction)
```

`Array.prototype.sort()` 會在原本的陣列將元素排序後，回傳排序完的陣列。也就是說，會改變原有陣列。

比較標準：

```javascript
function compare(a, b) {
  if (在某排序標準下 a 小於 b) {
    return -1;
  }
  if (在某排序標準下 a 大於 b) {
    return 1;
  }
  // a 必須等於 b
  return 0;
}
```

按照發明家的出生年份，從大到小排序：

```javascript
const inventors = [
  { first: 'Albert', last: 'Einstein', year: 1879, passed: 1955 },
  { first: 'Isaac', last: 'Newton', year: 1643, passed: 1727 },
  { first: 'Galileo', last: 'Galilei', year: 1564, passed: 1642 },
  { first: 'Marie', last: 'Curie', year: 1867, passed: 1934 },
  { first: 'Johannes', last: 'Kepler', year: 1571, passed: 1630 },
  { first: 'Nicolaus', last: 'Copernicus', year: 1473, passed: 1543 },
  { first: 'Max', last: 'Planck', year: 1858, passed: 1947 }
];

const oldest = inventors.sort((a, b) => {
  if (a.year > b.year) {
    return 1;
  } else {
    return -1;
  }
});
```

可以使用三元運算子：

```javascript
const oldest = inventors.sort((a, b) => a.year > b.year ? 1 : -1);
```

如果只是比較數字不是字串，可以直接用 `a-b`：

```javascript
const oldest = inventors.sort((a, b) => a.year - b.year); // 升冪排序
const oldest = inventors.sort((a, b) => b.year - a.year); // 降冪排序
```

參考資料：[MDN - Array.prototype.sort()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

## console 用法

最常用的就是 `console.log()`，但如果要讓資料按照表格輸出可以用 `console.table()`
