### Chapter 9 - 圖形 ( Graph )

```javascript
function Graph () {
  this.vertices = [];
  this.adjList = new Dictionary();
}

Graph.prototype = {
  
  addVertex: function (v) {
    this.vertices.push(v);
    this.adjList.set(v, []);
  },
  
  addEdge: function (v, w) {
    this.adjList.get(v).push(w);
    this.adjList.get(w).push(v);
  },
    
  toString: function () {
    var adjList = this.adjList;
    return adjList.keys().reduce(function(tot, key){      
      return tot + key + ' -> '+ adjList.get(key).join(' ') + '\n';      
    }, '\n');
  },
  
  /*
  bfs: function (v, callback) {
    var vertices = this.vertices;
    var adjList = this.adjList;
    var initializeColor = function () {
      var color = {};
      vertices.forEach(function(v){
        color[v] = 'white';
      });
      return color;
    };
    var color = initializeColor(),
        queue = new Queue();
    
    queue.enqueue(v); // 添加起始頂點
    while (!queue.isEmpty()) {
      var u = queue.dequeue(),
          neighbors = adjList.get(u);
    }    
  }
  */
};
```

Demo:

```javascript
var graph = new Graph();
var myVertices = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I'];
myVertices.forEach(function(v){
  graph.addVertex(v);
});
graph.addEdge('A', 'B');
graph.addEdge('A', 'C');
graph.addEdge('A', 'D');
graph.addEdge('C', 'D');
graph.addEdge('C', 'G');
graph.addEdge('D', 'G');
graph.addEdge('D', 'H');
graph.addEdge('B', 'E');
graph.addEdge('B', 'F');
graph.addEdge('E', 'I');


console.log(graph.toString());
```