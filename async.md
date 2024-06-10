## JavaScript  :是一個「非同步」的語言。
```
- 同步(sync)
必須先完成 A 才能做 B、C、D ...的運作方式我們就會把它稱作「同步」
EX.領完食材原料之後，一樣會有青菜、番茄需要處理。
因為只有一個廚師，所以要嘛先處理青菜、要嘛先處理番茄，必須先弄完一項之後再去處理另一項，整個流程會被前一個步驟卡住。

- 非同步(async)
理事件的流程不會被「卡住」，就是非同步 (Asynchronous) 的概念。我不用等待 A 做完才做 B、C，而是這三個事情可以同時發送出去。 
EX.不需要等到青菜切完才能處理番茄。
而是在收到食材的同時，負責青菜的朋友就去處理青菜，負責番茄的朋友就去處理番茄。
```
`Promise`按字面上的翻譯就是「承諾、約定」之意，回傳的結果要嘛是「完成」，要嘛是「拒絕」。
```js
const myFirstPromise = new Promise((resolve, reject) => {
  resolve(someValue);         // 完成
  reject("failure reason");   // 拒絕
});
```
要提供一個函式 `promise` 功能，讓它回傳一個 `promise` 物件即可：
```js
function myAsyncFunction(url) {
  return new Promise((resolve, reject) => {
    // resolve() or reject()
  });
};
```
當 `Promise` 被完成的時候，我們就可以呼叫 `resolve()`，然後將取得的資料傳遞出去。 或是說想要拒絕 `Promise` ，當個完全沒有信用的人， 那麼就呼叫 reject() 來拒絕。
```
一般來說， `Promise` 物件會有這幾種狀態：

- pending: 初始狀態，不是 fulfilled 或 rejected。
- fulfilled: 表示操作成功地完成。
- rejected: 表示操作失敗。
```

如果我們需要依序串連執行多個 `promise` 功能的話，可以透過 `.then()` 來做到。

以剛剛的 funcA, funcB, funcC 來當範例，我們將這三個函式分別透過 Promise 包裝：
```js
function funcA(){
  return new Promise(function(resolve, reject){
    window.setTimeout(function(){
      console.log('A');
      resolve('A');
    }, (Math.random() + 1) * 1000);
  });
}

function funcB(){
  return new Promise(function(resolve, reject){
    window.setTimeout(function(){
      console.log('B');
      resolve('B');
    }, (Math.random() + 1) * 1000);
  });
}

function funcC(){
  return new Promise(function(resolve, reject){
    window.setTimeout(function(){
      console.log('C');
      resolve('C');
    }, (Math.random() + 1) * 1000);
  });
}
```
### 同步 
最後透過呼叫
```js
funcA().then(funcB).then(funcC);
```
就可以做到等 `funcA()` 被 「resolve」之後再執行 `funcB()`，然後 resolve 再執行 `funcC()` 的順序了。
### 非同步 
```js
Promise.all([funcA(), funcB(), funcC()])
       .then(function(){ console.log('上菜'); });
```
async 和 await 是 JavaScript 中處理非同步操作的語法糖，讓非同步代碼看起來像同步代碼，從而更容易閱讀和維護。以下是詳細的解釋和使用範例。

## async 函數
async 關鍵字用於定義一個非同步函數。
非同步函數會隱式地返回一個 `Promise`，即使你在函數中返回的是一個普通值，該值也會被 `Promise` 包裝。

## await 關鍵字
await 只能在 async 函數中使用。
它會暫停 async 函數的執行，直到 await 後面的 Promise 完成（解決或拒絕）。

```js
// 定義一個 async 函數
async function fetchData() {
  // 使用 await 等待 Promise 完成
  let response = await fetch('https://api.example.com/data');
  let data = await response.json();
  return data;
}

// 調用 async 函數
fetchData().then(data => {
  console.log(data);
}).catch(error => {
  console.error(error);
});
```
在這個範例中，`fetchData` 是一個非同步函數，使用` await` 關鍵字來等待 `fetch` 和 `response.json()` 的完成。



