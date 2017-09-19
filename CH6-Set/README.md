### Chapter 6 - 集合 ( Set )

集合是由一組無序且唯一 (不能重複) 的項目所組成的一種資料結構。  
  
兩集合間常見的操作有:
* 聯集 (union)        - 包含兩集合所有元素的新集合。
* 交集 (intersection) - 包含兩集合中共有元素的新集合。
* 差集 (difference)   - 包含存在於一集合且不存在於另一集合的元素的新集合。
* 子集 (subset)       - 驗證一集合是否是另一集合的子集。


```javascript
function Set () {
  var items = {};
  if (arguments.length > 0) {
    Array.prototype.forEach.call(arguments, function(val){
      items[val] = val;
    });
  }
  this.items = items;
}

Set.prototype = {
  /**
   * Check a value in the set or not
   * 
   * @method has
   * @param  {*} value to check
   * @return {Boolean} to specify exist or not
   */
  has: function (value) {
    return this.items.hasOwnProperty(value);
  },
  /**
   * Add a value to the set
   * 
   * @method add
   * @param  {*} value to add
   * @return {Boolean} to specify success or not
   */  
  add: function (value) {
    if (!this.has(value)) {
      this.items[value] = value;
      return true;
    }
    return false;
  },
  /**
   * Remove a value from the set
   * 
   * @method remove
   * @param  {*} value to remove
   * @return {Boolean} to specify success or not
   */    
  remove: function (value) {
    if (this.has(value)) {
      delete this.items[value];
      return true;
    }
    return false;
  },
  /**
   * Clear the set to empty
   * 
   * @method clear
   */      
  clear: function () {
    this.items = {};
  },
  /**
   * Get the size of the set
   * 
   * @method size   
   * @return {Number} to specify the size of the set
   */      
  size: function () {
    return Object.keys(this.items).length;
  },
  /**
   * Get all values in the set
   * 
   * @method values
   * @return {Array} which contain all values of the set
   */        
  values: function () {
    return Object.keys(this.items);
  },
  /**
   * Get union set of this set and other set
   * 
   * @method union
   * @param {Set} otherSet
   * @return {Set} which is the union result
   */          
  union: function (otherSet) {
    var unionSet = new Set();
    
    this.values().forEach(function(val){
      unionSet.add(val);
    });
    otherSet.values().forEach(function(val){
      unionSet.add(val);
    });    
    
    return unionSet;
  },
  /**
   * Get intersection set of this set and other set
   * 
   * @method intersection
   * @param {Set} otherSet
   * @return {Set} which is the intersection result
   */    
  intersection: function (otherSet) {
    var intersectionSet = new Set();
    
    this.values().forEach(function(val){
      if (otherSet.has(val)) {
        intersectionSet.add(val);
      }
    });
    
    return intersectionSet;
  },
  /**
   * Get difference set from other set to this set
   * 
   * @method difference
   * @param {Set} otherSet
   * @return {Set} which is the difference result
   */    
  difference: function (otherSet) {
    var differenceSet = new Set();
    
    this.values().forEach(function(val){
      if (!otherSet.has(val)) {
        differenceSet.add(val);
      }
    });
    
    return differenceSet;
  },
  /**
   * Check this set is the subset of other set or not
   * 
   * @method isSubsetOf
   * @param  {Set} otherSet
   * @return {Boolean} 
   */    
  isSubsetOf: function (otherSet) {
    if (this.size() > otherSet.size()) {
      return false;
    }
    return this.values().every(function(val){
      return otherSet.has(val);
    });
  }  
};
```

Demo:

```javascript
var setA = new Set(1, 2, 3);
var setB = new Set(3, 4, 5, 6);
var unionAB = setA.union(setB);
console.log(unionAB.values());   // ["1", "2", "3", "4", "5", "6"]

setA = new Set(1, 2, 3);
setB = new Set(2, 3, 4);
var interAB = setA.intersection(setB);
console.log(interAB.values());  // ["2", "3"]

setA = new Set(1, 2, 3);
setB = new Set(2, 3, 4);
var diffAB = setA.difference(setB);
console.log(diffAB.values());   // ["1"]

setA = new Set(1, 2);
setB = new Set(1, 2, 3);
var setC = new Set(2, 3, 4);
console.log(setA.isSubsetOf(setB));  // true
console.log(setA.isSubsetOf(setC));  // false
```