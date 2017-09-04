### underscore [http://underscorejs.org/#collections](http://underscorejs.org/#collections)

第三方库underscore提供了一套完善的函数式编程的接口。jQuery在加载时，会把自身绑定到唯一的全局变量$上，underscore与其类似，会把自身绑定到唯一的全局变量 _ 上。

**collections**

```
// jQuery中Array有map()和filter()方法，但是Object没有这些方法。
// underscore的map()还可以作用于Object：
// Array
_.map([1, 2, 3], (x) => x*x); // [1, 4, 9]
// Object
_.map({a:1, b:2, c:3}, (v,k) => k + '=' +v);// ['a=1','b=2','c=3']
```

map()/filter()当作用于Object时，传入的函数为function(value, key);第一个参数接收value，第二个参数接收key

- 对Object做map()操作的返回结果是Array
- _.mapObject返回Object
- 当集合的所有元素都满足条件时，_.every()函数返回true
- 当集合的至少一个元素满足条件时，_.some()函数返回true
- _.max(arr); 返回集合中最大的数
- _.min(arr); 返回集合中最小的数
- _.groupBy()把集合的元素按照key归类，key由传入的函数返回
- _.shuffle()用洗牌算法随机打乱一个集合
- _.sample()随机选择一个或多个元素

```
'use strict';
_.every([1, 4, 7, -3], (x) => x>0); // false
_.some([1, 4, 7, -3], (x) => x>0); // true
var arr = [3, 5, 7, 9];
_.max(arr); // 9
_.min(arr); // 3

// 空集合会返回-Infinity和Infinity，所以要先判断集合不为空：
_.max([])
-Infinity
_.min([])
Infinity

// 如果集合是Object,max()和min()只作用于value
_.max({ a: 1, b: 2, c: 3 }); // 3

var scores = [20, 81, 75, 40, 91, 59, 77, 66, 72, 88, 99];
var groups = _.groupBy(scores, function (x) {
    if (x < 60) {
        return 'C';
    } else if (x < 80) {
        return 'B';
    } else {
        return 'A';
    }
});
// 结果:
// {
//   A: [81, 91, 88, 99],
//   B: [75, 77, 66, 72],
//   C: [20, 40, 59]
// }

// 注意每次结果都不一样：
_.shuffle([1, 2, 3, 4, 5, 6]); // [3, 5, 4, 6, 2, 1]

// 注意每次结果都不一样：
// 随机选1个：
_.sample([1, 2, 3, 4, 5, 6]); // 2
// 随机选3个：
_.sample([1, 2, 3, 4, 5, 6], 3); // [6, 1, 4]
```

**Arrays**

* _.first(arr); 取第一个元素

* _.last(arr); 取最后一个元素

* _.flatten()把多层Array变成一个一维数组

* _.zip()把两个或者多个数组的所有元素按索引对齐，然后按索引合并成新数组

* _.unzio()与_.zip()相反

  ```
  var names = ['Adam', 'Lisa', 'Bart'];
  var scores = [85, 92, 59];
  _.zip(names, scores);
  // [['Adam', 85], ['Lisa', 92], ['Bart', 59]]

  var namesAndScores = [['Adam', 85], ['Lisa', 92], ['Bart', 59]];
  _.unzip(namesAndScores);
  // [['Adam', 'Lisa', 'Bart'], [85, 92, 59]]
  ```

* _.object()把两个或多个数组的所有元素按索引对齐，然后按索引合并成Object

  ```
  var names = ['Adam', 'Lisa', 'Bart'];
  var scores = [85, 92, 59];
  _.object(names, scores);
  // {Adam: 85, Lisa: 92, Bart: 59}
  ```

* _.range()快速生成一个序列，不再需要用for循环实现

  ```
  // 从0开始小于10:
  _.range(10); // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

  // 从1开始小于11：
  _.range(1, 11); // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

  // 从0开始小于30，步长5:
  _.range(0, 30, 5); // [0, 5, 10, 15, 20, 25]

  // 从0开始大于-10，步长-1:
  _.range(0, -10, -1); // [0, -1, -2, -3, -4, -5, -6, -7, -8, -9]
  ```

  ​