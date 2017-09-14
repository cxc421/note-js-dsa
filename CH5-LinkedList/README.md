### Chapter 2 - 鏈結串列 ( Linked List )

#### 單向鏈結串列

```javascript
var LinkedList = (function(){
  var Node = function (element) {
    this.element = element;
    this.next = null;
  };
  function LinkedList () {
    this.length = 0;
    this.head = null;
  }
  LinkedList.prototype = {
    append: function (element) {
      var node = new Node(element);
      var current;
      
      if (this.head === null) {
        this.head = node;
      }
      else {
        current = this.head;
        // 串列迴圈，直到找到最後一項
        while (current.next) {
          current = current.next;
        }
        // 找到最後一項後，將其 next 指向 node，建立連結
        current.next = node;
      }
      this.length++;
    },
    removeAt: function (position) {
      var current = this.head;
      var index = 0;
      var previous;
      
      // 越界檢查
      if (position < 0 || position >= this.length) {
        return null;
      }
      
      if (position === 0) {
        // 移除第一項
        this.head = current.next;
      }
      else {
        while (index++ < position) {
          previous = current;
          current = current.next;
        }
        // 移除第 position 項
        previous.next = current.next;
      }          
      this.length--;
      return current.element;
    },
    insert: function (position, element) {
      var node = new Node(element);
      var current = this.head;
      var index = 0;
      var previous;
      
      // 越界檢查
      if (position < 0 || position > this.length) {
        return false;
      }
      
      if (position === 0) {
        node.next = current;
        this.head = node;
      }
      else {
        while (index++ < position) {
          previous = current;
          current = current.next;
        }
        node.next = current;
        previous.next = node;        
      }
      this.length++;
      return true;
    },
    toString: function () {
      var current = this.head,
          arr = [];
      
      while (current) {
        arr.push(current.element);
        current = current.next;
      }
      return arr.join(',');
    },
    indexOf: function (element) {
      var current = this.head,
          index = 0;
      while (current) {
        if (element === current.element) {
          return index;
        }
        index++;
        current = current.next;
      }
      return -1;
    },
    remove: function (element) {
      var index = this.indexOf(element);
      return this.removeAt(index);
    },
    isEmpty: function () {
      return (this.length === 0);
    },
    size: function () {
      return this.length;
    },
    getHead: function () {
      return head;
    },
    print: function () {
      console.log(this.toString());
    }
  };
  return LinkedList;
})();

```

#### 雙向鏈結串列

```javascript
var DoublyLinkedList = (function(){
  var Node = function (element) {
    this.element = element;
    this.next = null;
    this.prev = null;
  };
  function DoublyLinkedList () {
    this.length = 0;
    this.head = null;
    this.tail = null;
  }
  DoublyLinkedList.prototype = {
    
    getByPos: function (position) {
      var index, current;      
      // 檢查越界
      if (position < 0 || position >= this.length) {
        return null;
      }
      if (position > this.length / 2) {
        index = this.length - 1;
        current = this.tail;
        while (index-- > position) {
          current = current.prev;
        }        
      }
      else {
        index = 0;
        current = this.head;
        while (index++ < position) {
          current = current.next;
        }
      }
      return current;
    },
    
    append: function (element) {
      var node    = new Node(element);          
      
      if (this.length === 0) {
        this.head = node;
        this.tail = node;
      }
      else {
        this.tail.next = node;
        node.prev = this.tail;
        this.tail = node;
      }
      this.length++;
    },
    
    insert: function (position, element) {
      var node    = new Node(element),                    
          previous, current;      
      // 檢查越界
      if (position < 0 || position > this.length) {
        return false;
      }
      // 從開始位置添加
      if (position === 0) {
        if (!this.head) {
          this.head = node;
          this.tail = node;
        }
        else {
          current = this.head;
          node.next = current;
          current.prev = node;
          this.head = node;
        }
      }
      // 從最後位置添加
      else if (position === this.length) {
        current = this.tail;
        current.next = node;
        node.prev = current;
        this.tail = node;
        
      }
      // 從中間位置添加
      else {
        current  = this.getByPos(position);
        previous = current.prev;
        
        node.next = current;
        node.prev = previous;
        current.prev = node;
        previous.next = node;
      }
      this.length ++;      
      return true;
    },
    
    removeAt: function (position) {      
      var current, previous;
      // 檢查越界
      if (position < 0 || position >= this.length ) {
        return null;
      }
      // 移除第一項
      if (position === 0) {
        current = this.head;
        this.head = current.next;
        if (this.length === 1) {
          this.tail = null;
        }
        else {
          this.head.prev = null;
        }
      }
      // 移除最後一項
      else if (position === this.length - 1) {
        current = this.tail;
        this.tail = current.prev;
        this.tail.next = null;
      }
      // 移除中間項
      else {
        current = this.getByPos(position);
        previous = current.prev;
        
        previous.next = current.next;
        current.next.prev = previous;
      }
      this.length --;
      return current.element;
    },
    
    indexOf: function (element) {
      var current = this.head,
          index = 0;      
      while(current) {
        if (current.element === element) {
          return index;
        }
        index++;
        current = current.next;        
      }
      return -1;
    },
    
    remove: function (element) {
      var index = this.indexOf(element);
      return this.removeAt(index);
    },
    
    isEmpty: function () {
      return (this.length === 0);
    },
    
    size: function () {
      return this.length;
    },
    
    toString: function () {
      var current = this.head,
          arr = [];
      while (current) {
        arr.push(current.element);
        current = current.next;
      }
      return arr.join(', ');
    },
    
    inverseToString: function () {
      var current = this.tail,
          arr = [];
      while (current) {
        arr.push(current.element);
        current = current.prev;
      }
      return arr.join(', ');      
    },
    
    print: function () {
      console.log(this.toString());
    },
    
    printInverse: function () {
      console.log(this.inverseToString());
    },
    
    getHead: function () {
      return this.head;
    },
    
    getTail: function () {
      return this.tail;
    }
    
  };
  
  return DoublyLinkedList;
})();
```