### Chapter 5 - 鏈結串列 ( Linked List )

Linked List 相較於 Array，元素的記憶體位置不需要連續。  
因此在 list 中間新增或移除元素時，不需要移動其他元素的位置，這會比 Array 有效率。  
但缺點就是存取元素時只能從 head 或 tail 循序的尋找，存取速度會比 Array 慢。  

#### 單向鏈結串列   

單向鏈結每個元素含有一個 element 代表此元素的值，以及一個 next 代表下一個元素 ( 最後一個元素的 next 值為 null )。  
鏈結會有一個 head 指向第一個元素，因此可利用 head 及每個元素的 next 來存取所有元素。  

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
      return this.head;
    },
    print: function () {
      console.log(this.toString());
    }
  };
  return LinkedList;
})();

```

#### 雙向鏈結串列   

雙鏈結每個元素含有一個 element 代表此元素的值，一個 next 代表下一個元素 ( 最後一個元素的 next 值為 null )，一個 prev 代表上一個元素 ( 第一個元素的 prev 值為 null )。  
鏈結會有一個 head 指向第一個元素，以及一個 tail 指向最後一個元素。  
因此可利用 head 搭配每個元素的 next 來存取所有元素，或是用 tail 搭配每個元素的 prev 存取所有元素。  
這樣在存取特定位置的元素時，可以先判斷比較靠近 head 或是 tail，再決定從哪個方向循序存取，以提升存取速度。

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

#### 環狀鏈結串列   

環狀鏈結串列可以是單向或是雙向的。  
若是單向環狀鏈結串列，最後一個元素的 next 不是 null 而是指向第一個元素。  
若是雙向環狀鏈結串列，最後一個元素的 next 不是 null 而是指向第一個元素，第一個元素的 prev 也不是 null 而是指向最後一個元素。  
以下展示單向環狀鏈結的程式碼。

```javascript
function CircularLinkedList() {

    let Node = function(element){

        this.element = element;
        this.next = null;
    };

    let length = 0;
    let head = null;

    this.append = function(element){

        let node = new Node(element),
            current;

        if (head === null){ //first node on list
            head = node;
        } else {

            current = head;

            //loop the list until find last item
            while(current.next !== head){ //last element will be head instead of NULL
                current = current.next;
            }

            //get last item and assign next to added item to make the link
            current.next = node;
        }

        //set node.next to head - to have circular list
        node.next = head;

        length++; //update size of list
    };

    this.insert = function(position, element){

        //check for out-of-bounds values
        if (position >= 0 && position <= length){

            let node = new Node(element),
                current = head,
                previous,
                index = 0;

            if (position === 0){ //add on first position
                
                if(!head){ // if no node  in list
                    head = node;
                    node.next = head;
                }else{
                    node.next = current;

                    //update last element
                    while(current.next !== head){ //last element will be head instead of NULL
                        current = current.next;
                    }

                    head = node;
                    current.next = head;
                }
                

            } else {
                while (index++ < position){
                    previous = current;
                    current = current.next;
                }
                node.next = current;
                previous.next = node;
            }

            length++; //update size of list

            return true;

        } else {
            return false;
        }
    };

    this.removeAt = function(position){

        //check for out-of-bounds values
        if (position > -1 && position < length){

            let current = head,
                previous,
                index = 0;

            //removing first item
            if (position === 0){

                while(current.next !== head){ //needs to update last element first
                    current = current.next;
                }

                head = head.next;
                current.next = head;

            } else { //no need to update last element for circular list

                while (index++ < position){

                    previous = current;
                    current = current.next;
                }

                //link previous with current's next - skip it to remove
                previous.next = current.next;
            }

            length--;

            return current.element;

        } else {
            return null;
        }
    };

    this.remove = function(element){

        let index = this.indexOf(element);
        return this.removeAt(index);
    };

    this.indexOf = function(element){

        let current = head,
            index = -1;

        //check first item
        if (element == current.element){
            return 0;
        }

        index++;

        //check in the middle of the list
        while(current.next !== head){

            if (element == current.element){
                return index;
            }

            current = current.next;
            index++;
        }

        //check last item
        if (element == current.element){
            return index;
        }

        return -1;
    };

    this.isEmpty = function() {
        return length === 0;
    };

    this.size = function() {
        return length;
    };

    this.getHead = function(){
        return head;
    };

    this.toString = function(){

        let current = head,
            s = current.element;

        while(current.next !== head){
            current = current.next;
            s += ', ' + current.element;
        }

        return s.toString();
    };

    this.print = function(){
        console.log(this.toString());
    };
}

```