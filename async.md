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
function sleep(ms) {
  // 創建並返回一個Promise，該Promise會在指定毫秒數後解決
  return new Promise((resolve, reject) => {
    setTimeout(resolve, ms); // 設置定時器，在ms毫秒後調用resolve
  });
}

// 使用範例
async function example() {
  console.time();       // 開始計時
  console.log('Start'); // 在控制台輸出 'Start'
  await sleep(2000);    // 暫停執行2秒
  console.log('End');   // 2秒後，在控制台輸出 'End'
  console.timeEnd();    // 結束計時並輸出經過的時間
}

example(); // 調用example函數，開始執行上述步驟

```

## 當調用example函數時，以下步驟會依次發生：

1.開始計時。
2.輸出'Start'。
3.暫停執行2秒（因為sleep(2000)返回的Promise在2秒後解決）。
4.2秒後，輸出'End'。
5.結束計時並輸出經過的時間。

## 運行結果
```js
Start
End
default: 2002.000ms  // (這個數字大約是2秒，具體數值可能略有差異)
```



