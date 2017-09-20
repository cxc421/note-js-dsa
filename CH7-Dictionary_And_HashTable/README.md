### Chapter 7 - 字典和雜湊表 ( Dictionary and Hash Table )

#### 字典

字典用 [key, value] 對來存取元素，key 不可以重複。  

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