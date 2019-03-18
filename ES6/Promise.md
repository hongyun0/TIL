## Promise 객체
https://joshua1988.github.io/web-development/javascript/promise-for-beginners/
### Promise의 세 가지 상태
1. new Promise() -> Pending(대기)
2. resolve() -> Fulfilled(이행) -> .then() 실행
3. reject() -> Rejected(실패) -> .catch() 실행  
```js
function getData() {
  return new Promise(function (resolve, reject) {      // 1. Pending(대기)
    $.get('url 주소/products/1', function (response) {
      if (response) {
        resolve(response);                             // 2. Fulfilled(이행)
      }
      reject(new Error("Request is failed"));          // 3. Rejected(실패)
    });
  });
}
```

### .then().catch()  
Promise.prototype.then() / Promise.prototype.catch()  
리턴값으로 Promise를 반환하기 때문에 chaining이 가능함.
- **then(성공 시 콜백 함수[, 실패 시 콜백 함수])**  
에러 처리 콜백 함수를 두 번째 인자로 주는 것보다, catch()를 이용해 분리하는 게 에러 trace를 더 확실하게 볼 수 있음.
두 번째 인자로 줄 경우, then()의 콜백 함수 안에서 에러가 발생하는 경우 잡아낼 수 없음.
- **catch(예외 시 콜백 함수)**  
```js
getData().then(function (result) {
  console.log(result);
  throw new Error("Error in then()");
}).catch(function (err) {
  console.log('then error : ', err); 
  // then error :  Error: Error in then()
});
```
