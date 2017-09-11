### Chapter 2 - 陣列 ( Array )

EX: 斐波那契數列
```javascript
var fibonacci = [];
var NUM = 6;
fibonacci[0] = 0;
fibonacci[1] = 1;
for (var i = 2; i < NUM; i++) {
    fibonacci[i] = fibonacci[i - 1] + fibonacci[i - 2];
}
console.log(fibonacci); // [0, 1, 1, 2, 3, 5]
```

#### 添加/刪除元素   
EX:
```javascript
var numbers = [1, 2, 3];
var num;
numbers.push(4, 5);            // 添加元素 (4, 5) 到陣列尾端 
numbers.unshift(-1, 0);        // 添加元素 (-1, 0) 到陣列首位
num = numbers.pop();           // 移除陣列尾端元素 5, 並存放到 num
num = numbers.shift();         // 移除陣列首位元素 -1, 並存放到 num
numbers.splice(1, 3);          // 從第1個位置移除3個元素, 也就是移除 numbers[1] ~ numbers[3] = 1 ~ 3
numbers.splice(1, 0, 1, 2, 3); // 從第1個位置移除0個元素, 再添加 1, 2, 3
console.log(numbers);          // [0, 1, 2, 3, 4]
```

#### 多維陣列

二維陣列
```javascript
var matrix2x6 = [];
matrix2x6[0] = [72, 75, 79, 79, 81, 81];
matrix2x6[1] = [81, 75, 75, 75, 73, 72];
```

三維陣列
```javascript
var matrix3x3 = [];
for (var i=0; i<3; i++) {
    matrix3x3[i] = [];
    for (var j=0; j<3; j++) {
        matrix3x3[i][j] = [];
        for (var k=0; k<3; k++) {
          matrix3x3[i][j][k] = i + j + z;
        }
    }
}
```

列印陣列
```javascript
function printMatrix(myMatrix, prefix) {
  prefix = prefix || '';
  if (!Array.isArray(myMatrix)) {
    return console.log(prefix + "=" + myMatrix);
  }
  for (var i=0; i<myMatrix.length; i++) {
    printMatrix(myMatrix[i], prefix+'[' + i + ']');
  }  
}
printMatrix(matrix2x6, 'matrix2x6');
printMatrix(matrix3x3, 'matrix3x3');
```

#### 常用陣列方法

##### concat: 向一個陣列合併陣列、物件或元素

```javascript
var arr = [1,2,3];
var newArr = arr.concat(0, [4,5,6]);
console.log(newArr); // [1,2,3,0,4,5,6];
```
##### every: 檢查一陣列是否全部通過某測試
##### some : 檢查一陣列是否至少有一個元素通過測驗

EX:
```javascript
var arr = [-1, 1, -5];
var test = function (val) {
    return val>0;
};
var every_res = arr.every(test); // false
var some_res  = arr.some(test);  // true
```
##### forEach: 迭代整個陣列，無回傳值 (undefined)。
##### map: 迭代整個陣列，根據函式的 return 值，回傳一長度相同的新陣列。
##### filter: 迭代整個陣列，回傳的新陣列由函式 retun true 的元素組成。
##### reduce: 迭代整個陣列，回傳累加的值。
EX:
```javascript
var arr = [1, 2, 3, 4];
var isEven = function (x) {
    return (x % 2 === 0);
};
var res_forEach = arr.forEach(isEven); // undefined;
var res_map     = arr.map(isEven);     // [false, true, false, true]
var res_filter  = arr.filter(isEven);  // [2, 4]
``` 

#### 排序  

Javascript 中預設的 sort()，如果沒有特別指定如何排序的函式，會以字串的 unicode 數值排序。
```javascript
var arr = [1, 5, 10];
arr.sort();
console.log(arr); // [1, 10, 5]
```
```javascript
var arr = ['A', 'a', 'J', 'j'];
arr.sort();
console.log(arr); // ['A', 'J', 'a', 'j'];
console.log('A'.charCodeAt(0)); // 65
console.log('J'.charCodeAt(0)); // 74
console.log('a'.charCodeAt(0)); // 97
console.log('j'.charCodeAt(0)); // 106
```  

指定排序函式，sort()會根據 return 數值為正數、負數、或零，來決定數值的先後順序.
```javascript
var arr = [1, 5, 2, 10];
var sortFunc = function (a, b) {  // 一般升冪函式定義
  return a - b;
};
arr.sort(sortFunc);
console.log(arr); // [1, 2, 5, 10]
```  

如果對有抑音符號的字元做排序，可以用 localCompare 實作
```javascript
var names2 = ['Maève', 'Maeve'];
console.log(names2.sort(function(a, b){
    return a.localeCompare(b);
}));
```

#### 搜尋  

##### indexOf: 找出第一個匹配元素的索引
##### lastIndexOf: 找出最後一個匹配元素的索引
