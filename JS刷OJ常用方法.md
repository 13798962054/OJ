## 1、获取用户输入
```JS
process.stdin.resume();
process.stdin.setEncoding('utf-8');
let input = '';
process.stdin.on('data', (data) => {
  input += data;
});
process.stdin.on('end', () => {
    let inputArray = input.split('\n');
    console.log(inputArray)

  
    process.exit();
});
```
## 2、字符串转数组
```js
'123456'.split('');
```
### 3、数组转字符串
```js
[1,2,3,4,5].join('');
```
### 4、四舍五入
```js
45.256.toFixed(2);
// 45.26
```
