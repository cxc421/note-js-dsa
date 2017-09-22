### Chapter 8 - 樹 ( Tree )

樹是一種非循序的資料結構，對於存放需要快速尋找的資料非常有用。
現實中常見的例子有家譜，或是組織圖。

術語:
* 樹中有一系列存在父子關係的節點。
* 樹中有唯一一個 "根節點"，它沒有父節點。
* 沒有子節點的節點稱為 "葉節點" 或 "外部節點"。
* 至少有一個子節點的節點稱為 "內部節點"。
* 祖先節點為包含父節點及其上的所有節點，後代節點為包含子節點及其下的所有節點。
* 每個節點有一個深度的屬性，其取決於祖先節點的數量。
  舉例來說，根節點的深度為 0。
* 樹的高度為其所有節點深度的最大值。

##### 二元樹及二元搜尋樹

二元樹: 每個節點最多只能含有兩個子節點。  
二元搜尋樹 (BST): 二元樹的一種，但左側的後代節點只能存放比該節點小的值，右側的後代節點只能存放比該節點大(或相等)的值。

以下展示二元搜尋樹的程式碼:  
// ToDo: 程式碼 method 簡介


```javascript
var BinarySearchTree = (function(){
  
  var Node = function (key) {
    this.key = key;
    this.left = null;
    this.right = null;
  };
  
  function BinarySearchTree () {
    this.root = null;
  }
  BinarySearchTree.prototype = {
    /**
     *  插入新節點
     */
    insert: function (key) {
      var newNode = new Node(key);
      
      var insertNode = function (node, newNode) {
        if (newNode.key < node.key) {
          if (node.left === null) {
            node.left = newNode;
          }
          else {
            insertNode(node.left, newNode);
          }
        }
        else {
          if (node.right === null) {
            node.right = newNode;
          }
          else {
            insertNode(node.right, newNode);
          }
        }
      };
      
      if (this.root === null) {
        this.root = newNode;
      }
      else {
        insertNode(this.root, newNode);
      }
      
    },
    /**
     *  透過中序歷遍所有節點
     */    
    inOrderTraverse: function (callback) {
      var inOrderTraverseNode = function (node, callback, depth) {
        if (node !== null) {
          inOrderTraverseNode(node.left, callback, depth + 1);
          callback(node.key, depth);
          inOrderTraverseNode(node.right, callback, depth + 1);
        }
      };            
      inOrderTraverseNode(this.root, callback, 0);
    },
    /**
     *  透過先序歷遍所有節點
     */        
    preOrderTraverse: function (callback) {
      var preOrderTraverseNode = function (node, callback, depth) {
        if (node !== null) {
          callback(node.key, depth);
          preOrderTraverseNode(node.left, callback, depth + 1);
          preOrderTraverseNode(node.right, callback, depth + 1);
        }
      };
      preOrderTraverseNode(this.root, callback, 0);
    },
    /**
     *  透過後序歷遍所有節點
     */            
    postOrderTraverse: function (callback) {
      var postOrederTraverseNode = function (node, callback, depth) {
        if (node !== null) {
          postOrederTraverseNode(node.left, callback, depth + 1);
          postOrederTraverseNode(node.right, callback, depth + 1);
          callback(node.key, depth);
        }
      };
      postOrederTraverseNode(this.root, callback, 0);
    },
    /**
     *  返回樹中的最小值
     */            
    min: function () {
      var minNode = function (node) {
        if (node === null) {
          return null;
        }
        if (node.left === null) {
          return node.key;
        }
        return minNode(node.left);
      };
      return minNode(this.root);
    },
    /**
     *  返回樹中的最大值
     */                    
    max: function () {
      var maxNode = function (node) {
        if (node === null) {
          return null;
        }
        if (node.right === null) {
          return node.key;
        }
        return maxNode(node.right);
      }
      return maxNode(this.root);
    },
    /**
     *  檢查是否任意 key 存在於樹中
     */                
    search: function (key) {
      var searchNode = function (node, key) {
        if (node === null) {
          return false;
        }
        if (key < node.key) {
          return searchNode(node.left, key)
        }
        if (key > node.key) {
          return searchNode(node.right, key);
        }
        return true;
      };
      return searchNode(this.root, key);
    },    
    /**
     *  移除某節點
     */                
    remove: function (key) {
      var findMinKey = function (node) {
        if (node === null) {
          return null;
        }
        if (node.left === null) {
          return node.key;
        }
        return findMinKey(node.left);
      };            
      var removeNode = function (node, key) {
        if (node !== null) {
          if (key < node.key) {
            node.left = removeNode(node.left, key);
          }
          else if (key > node.key) {
            node.right = removeNode(node.right, key);
          }
          else {
            if (node.left === null && node.right === null) {
              node = null;
            }
            else if (node.left === null) {
              node = node.right;
            }
            else if (node.right === null) {
              node = node.left;
            }
            else {
              var minKeyOfRightSubTree = findMinKey(node.right);
              node.key = minKeyOfRightSubTree;
              node.right = removeNode(node.right, minKeyOfRightSubTree);
            }
          }
        }
        return node;
      };                       
      this.root = removeNode(this.root, key);
    }
  };
  
  return BinarySearchTree;
})();
```

// ToDo: AVL 樹, 紅黑樹, 堆積樹。
