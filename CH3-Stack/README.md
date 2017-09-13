### Chapter 2 - 堆疊 ( Stack )

堆疊是一種遵從後進先出 (LIFO, Last In First Out) 的有序集合.

#### 建立堆疊 

```javascript
function Stack () {
  this.items = [];
}

Stack.prototype = {
  push: function() {    
    this.items.push.apply(this.items, arguments);
  },
  pop: function() {
    return this.items.pop();
  },
  // peek: 只回傳堆疊最上層的元素，不移除
  peek: function() {
    return this.items[this.items.length - 1];
  },
  isEmpty: function() {
    return this.items.length === 0;
  },
  size: function() {
    return this.items.length;
  },  
  clear: function() {
    this.items.length = 0;
  },
  print: function() {
    console.log(this.items);
  }
};
```

#### 應用例子: 十進位轉換成其他進位

概念: 依序除去 base，將餘數 push 到 stack，直到不可除，最後再 pop 出結果
```
ex:  10 = 5 * 2 + 0                                     // push(0) 
        = ( 2 * 2 + 1 ) * 2 + 0                         // push(1)
        = ( ( 1 * 2 + 0 ) * 2 + 1 ) * 2 + 0             // push(0)
        = ( ( ( 0 * 2 + 1 ) * 2 + 0 ) * 2 + 1 ) * 2 + 0 // push(1)
        = 1 * 2^3 + 0 * 2^2 + 1 * 2^1 + 0 * 2^0         // pop() all => "1010"
```

十進位轉二進位  

```javascript
function divideBy2 (decNumber) {
  var remStack = new Stack(),
      binaryString = '',
      rem;
  
  if (decNumber < 0) {
    throw new Error("Can't convert negative number");
  }
  if (decNumber === 0) {
    return '0';
  }
  
  while (decNumber > 0) {
    rem = (decNumber % 2);
    remStack.push(rem);
    decNumber = Math.floor(decNumber / 2);
  }
  while (!remStack.isEmpty()) {
    binaryString += remStack.pop().toString();
  }
  return binaryString;
}

console.log(divideBy2(0));   // "0"
console.log(divideBy2(233)); // "11101001"
console.log(divideBy2(10));  // "1010"
```

十進位轉其他進位
```javascript
function baseConverter(decNumber, base) {
  var remStack = new Stack(),
      baseString = '',
      digits = '0123456789ABCDEF',
      rem;
  
  if (decNumber < 0) {
    throw new Error("Can't convert negative number");
  }
  if (decNumber === 0) {
    return '0';
  }  
  
  while (decNumber > 0) {
    rem = decNumber % base;
    remStack.push(rem);
    decNumber = Math.floor(decNumber / base);
  }
  while (!remStack.isEmpty()) {
    baseString += digits[remStack.pop()];
  }
  return baseString;
}

console.log(baseConverter(100345, 2));   // "11000011111111001"
console.log(baseConverter(100345, 8));   // "303771"
console.log(baseConverter(100345, 16));  // "187F9"
```

#### 應用例子: 檢查括號是否對稱

```javascript
function matches(open, close){
    var opens = "([{",
        closers = ")]}";
    return opens.indexOf(open) == closers.indexOf(close);
}

function parenthesesChecker(symbols){

    var stack = new Stack(),
        balanced = true,
        index = 0,
        symbol, top;

    while (index < symbols.length && balanced){
        symbol = symbols.charAt(index);
        if (symbol == '('|| symbol == '[' || symbol == '{'){
            stack.push(symbol);
        } else {
            if (stack.isEmpty()){
                balanced = false;
            } else {
                top = stack.pop();
                if (!matches(top, symbol)){
                    balanced = false;
                }
            }
        }
        index++;
    }
    if (balanced && stack.isEmpty()){
        return true;
    }
    return false;
}

console.log(parenthesesChecker('{{([][])}()}'));  // true
console.log(parenthesesChecker('[{()]'));         // false
```

#### 應用例子: 河內塔

無 Stack 的例子:
```javascript
function hanoi(n, from, buffer, to) {
  if (n>0) {
    hanoi(n-1, from, to, buffer);
    console.log('move disk ' + n + ' from ' + from + ' to ' + to);
    hanoi(n-1, buffer, from, to);
  }
}

hanoi(3, 'A', 'B', 'C');
```

用 Stack 觀察的例子:
```javascript
function towerOfHanoi(n, from, to, helper){

    if (n > 0){
        towerOfHanoi(n-1, from, helper, to);
        to.push(from.pop());
        console.log('-----')
        console.log('Source: ' + from.items);
        console.log('Dest: ' + to.items);
        console.log('Helper: ' + helper.items);
        towerOfHanoi(n-1, helper, to, from);
    }
}

var source = new Stack();
source.push(3);
source.push(2);
source.push(1);

var dest = new Stack();
var helper = new Stack();

towerOfHanoi(3, source, dest, helper);
```


