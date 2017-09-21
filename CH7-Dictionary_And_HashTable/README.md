### Chapter 7 - 字典和雜湊表 ( Dictionary and Hash Table )

#### 字典

字典用 [鍵, 值] 對來存取元素，key 不可以重複。  

```javascript
function Dictionary() {
  this.items = {};
}

Dictionary.prototype = {
  
  has: function (key) {
    return this.items.hasOwnProperty(key);
  },
  
  set: function (key, value) {
    this.items[key] = value;
  },
  
  delete: function (key) {
    if (this.has(key)) {
      delete this.items[key];
      return true;
    }
    return false;
  },
  
  get: function (key) {
    return this.items[key];
  },
  
  values: function () {
    var items = this.items;
    return Object.keys(items).map(function(key){
      return items[key];
    });
  },
  
  clear: function () {
    this.items = {};
  },
  
  size: function () {
    return Object.keys(this.items).length;
  },
  
  keys: function () {
    return Object.keys(this.items);
  },
  
  each: function (fn) {
    var items = this.items;
    Object.keys(items).forEach(function(key){
      fn(key, items[key]);      
    });
  },
  
  getItems: function() {
    return this.items;
  }  
};
```

Demo:

```javascript
var dictionary = new Dictionary();

dictionary.set('Gandalf', 'gandalf@email.com');
dictionary.set('John', 'johnsnow@email.com');
dictionary.set('Tyrion', 'tyrion@email.com');

console.log(dictionary.has('Gandalf'));   //outputs true
console.log(dictionary.size());   //outputs 3

console.log(dictionary.keys()); //outputs ["Gandalf", "John", "Tyrion"]
console.log(dictionary.values()); //outputs ["gandalf@email.com", "johnsnow@email.com", "tyrion@email.com"]
console.log(dictionary.get('Tyrion')); //outputs tyrion@email.com

dictionary.delete('John');

console.log(dictionary.keys()); //outputs ["Gandalf", "Tyrion"]
console.log(dictionary.values()); //outputs ["gandalf@email.com", "tyrion@email.com"]

console.log(dictionary.getItems()); //Object {Gandalf: "gandalf@email.com", Tyrion: "tyrion@email.com"}
```

#### 雜湊表

雜湊表一樣是用 [鍵, 值] 對來存取資料。
與字典不同的是，實際儲存資料是存在陣列裡。
因此，需要一種將鍵 (key) 轉換成陣列位置的方法，這就是雜湊函數 (Hash Function)。
好的雜湊函數需要轉換速度快，而且要盡可能讓轉換出的位置不要有衝突. (不同 key 卻轉換出相同位置).
但如果真的發生了衝突，我們需要有備用解決衝突的方法，以下介紹兩種方法: 分離鏈結 及 線性探查 。  

##### 分離鏈結

分離鏈結的概念就是在另一個維度將衝突的元素儲存起來，這裡用 Linked List。  
以下故意用容易產生衝突的 lose-lose-hash-code 雜湊方法來實作以方便測試。
並且，以下展示的程式碼沒有考慮儲存相同鍵值的資料的狀況 (會全部儲存起來而不是更新)。  

```javascript
var HashTable = (function(){
  
  var loseloseHashCode = function (key) {    
    
    var hash = Array.prototype.reduce.call(key, function(tot, val){
      return tot + val.charCodeAt(0);
    }, 0);        
    
    return hash % 37;
  };
  
  function ValuePair (key, value) {
    this.key = key;
    this.value = value;
  }
  ValuePair.prototype.toString = function () {
    return '[' + this.key + ' - ' + this.value + ']';
  };
  
  function HashTable () {
    this.table = [];
  }  
  HashTable.prototype = {
    
    put: function (key, value) {
      var position = loseloseHashCode(key);
      console.log(position + ' - ' + key);
      
      if (this.table[position] === undefined) {
        this.table[position] = new LinkedList();
      }
      this.table[position].append( new ValuePair(key, value) );
    },
    
    get: function (key) {
      var position = loseloseHashCode(key);
      
      if (this.table[position] !== undefined && ! this.table[position].isEmpty()) {
        var current = this.table[position].getHead();
        do {
          if (current.element.key === key) {
            return current.element.value;
          }
          current = current.next;
        } while (current);
      }
      return undefined;
    },
    
    remove: function (key) {
      var position = loseloseHashCode(key);
      
      if (this.table[position] !== undefined && ! this.table[position].isEmpty()) {
        var current = this.table[position].getHead();
        do {
          if (current.element.key === key) {
            this.table[position].remove(current.element);
            return true;
          }
          current = current.next;
        } while (current);
      }
      return undefined;
    },
    
    print: function () {
      this.table.forEach(function(list, index){
        if (list !== undefined && !list.isEmpty()) {
          console.log(index +': ' + list.toString());
        }
      });
    }
    
  };
    
  return HashTable;  
})();
```


##### 線性探查

線性衝突的概念就是如果發現衝突，就往下繼續找其他位置，直到找到還沒被使用的位置。
這個方法在一般陣列長度是固定的程式語言比較容易失敗，因為位置有可能被用光。
另一方面，假設發生衝突的位置很靠陣列尾端，就算陣列前面有位置也不會發現。

```javascript
function HashLinearProbing(){

    var table = [];

    var ValuePair = function(key, value){
        this.key = key;
        this.value = value;

        this.toString = function() {
            return '[' + this.key + ' - ' + this.value + ']';
        }
    };

    var loseloseHashCode = function (key) {
        var hash = 0;
        for (var i = 0; i < key.length; i++) {
            hash += key.charCodeAt(i);
        }
        return hash % 37;
    };

    var hashCode = function(key){
        return loseloseHashCode(key);
    };

    this.put = function(key, value){
        var position = hashCode(key);
        console.log(position + ' - ' + key);

        if (table[position] == undefined) {
            table[position] = new ValuePair(key, value);
        } else {
            var index = ++position;
            while (table[index] != undefined){
                index++;
            }
            table[index] = new ValuePair(key, value);
        }
    };

    this.get = function(key) {
        var position = hashCode(key);

        if (table[position] !== undefined){
            if (table[position].key === key) {
                return table[position].value;
            } else {
                var index = ++position;
                while (table[index] !== undefined && (table[index] && table[index].key !== key)){
                    index++;
                }
                if (table[index] && table[index].key === key) {
                    return table[index].value;
                }
            }
        } else { //search for possible deleted value
            var index = ++position;
            while (table[index] == undefined || index == table.length ||
                (table[index] !== undefined && table[index] && table[index].key !== key)){
                index++;
            }
            if (table[index] && table[index].key === key) {
                return table[index].value;
            }
        }
        return undefined;
    };

    this.remove = function(key){
        var position = hashCode(key);

        if (table[position] !== undefined){
            if (table[position].key === key) {
                table[position] = undefined;
            } else {
                var index = ++position;
                while (table[index] === undefined || table[index].key !== key){
                    index++;
                }
                if (table[index].key === key) {
                    table[index] = undefined;
                }
            }
        }
    };

    this.print = function() {
        for (var i = 0; i < table.length; ++i) {
            if (table[i] !== undefined) {
                console.log(i + ' -> ' + table[i].toString());
            }
        }
    };
}
```

##### 更好的雜湊函數

這邊介紹一個有效率有比較不容易產生衝突的雜湊函數: djb2HashCode.

```javascript
var djb2HashCode = function (key) {
	var hash = 5381;
	for (var i = 0; i < key.length; i++) {
		hash = hash * 33 + key.charCodeAt(i);
	}
	return hash % 1013; // Assume size of hash table is smaller than 1000
};
```
