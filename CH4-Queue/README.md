### Chapter 2 - 佇列 ( Queue )

佇列是一種遵從先進先出 (FIFO, First In First Out) 的有序集合.

#### 建立佇列

```javascript
function Queue () {
  this.items = [];
}
Queue.prototype = {
	// enqueue: 新增元素
  enqueue: function () {
    this.items.push.apply(this.items, arguments);
  },
  // dequeue: 移出元素
  dequeue: function () {
    return this.items.shift();
  },
  // front: 檢查目前可移除的第一個元素
  front: function () {
    return this.items[0];
  },
  isEmpty: function () {
    return (this.items.length === 0);
  },
  size: function () {
    return this.items.length;
  },
  print: function () {
    console.log(this.items);    
  }
};
```

#### 優先佇列

除了 FIFO 之外，多考慮 priority 的數值來排序。  
如果 priority 較大的元素排較後面，稱為 最小優先佇列 (Min Priority Queue)。  
如果 priority 較大的元素排較前面，稱為 最大優先佇列 (Max Priority Queue)。  
以下展示最小優先佇列的寫法。

```javascript
function PriorityQueue () {
  Queue.call(this);
}
PriorityQueue.prototype = Object.create(Queue.prototype);
PriorityQueue.prototype.contructor = PriorityQueue;
PriorityQueue.prototype.enqueue = function (element, priority) {
  var queueElement = {
    element : element,
    priority: priority
  };
  if (this.isEmpty()) {
    this.items.push(queueElement);
  }
  else {
    var added = false;
    for (var i=0; i<this.items.length; i++) {
      if (queueElement.priority < this.items[i].priority) {
        this.items.splice(i, 0, queueElement);
        added = true;
        break;
      }
    }
    if (!added) {
      this.items.push(queueElement);
    }
  }
};
```

使用例子:

```javascript
var priorityQueue = new PriorityQueue();
priorityQueue.enqueue("John", 2);
priorityQueue.enqueue("Jack", 1);
priorityQueue.enqueue("Camila", 1);
priorityQueue.print();  // [{element: "Jack", priority: 1}, {element: "Camila", priority: 1}, {element: "John", priority: 2}]
```

#### 環狀佇列 - 燙手山芋遊戲

環狀佇列的概念，就是從佇列開頭移除的元素，又會被添加到佇列尾端。  
這可以用一個叫燙手山芋的遊戲 (Hot Potato) 來說明: 遊戲中孩子們圍成一圈，將一個物品盡快地傳給旁邊的人，當某一個時刻停止時，這時候物品在誰手裡，誰就被淘汰。  
以下用程式模擬這個遊戲，為了簡單起見先假設傳遞次數固定。

```javascript
function hotPotatoGame (nameList, num) {
  var queue = new Queue();
  var eliminated = '';
  queue.enqueue.apply(queue, nameList);    
  while(queue.size() > 1) {
    for (var i=0; i<num; i++) {
      queue.enqueue(queue.dequeue());
    }
    eliminated = queue.dequeue();
    console.log(eliminated + ' 在燙手山芋遊戲中被淘汰');
  }
  var winner = queue.dequeue();
  console.log('勝利者 : ' + winner);  
}
var names = ['John', 'Jack', 'Camila', 'Ingrid', 'Carl'];
var winner = hotPotatoGame(names, 7);
```
