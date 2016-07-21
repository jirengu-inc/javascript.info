> 译者: 曾安

接下来我们将讨论用数字索引来访问的常规数组。
数组通常用方括号【】来声明：

	var fruits = ["Apple", "Orange", "Donkey"]
可以通过方括号和数字索引来获取数组的元素。第一个元素的索引为0：

	var fruits = ["Apple", "Orange", "Donkey"]

	alert(fruits[0])
	alert(fruits[1])
	alert(fruits[2])
我们也可以检索它的长度

	var fruits = ["Apple", "Orange", "Donkey"]

	alert(fruits.length)
我们就创造了包含"Apple"、"Orange"和"Donkey"的数组。下一步将演示如何删除"Donkey"。
## pop()方法和push()方法
pop()方法会从数组末端移除最后一项元素，并返回这一元素。
下面的例子演示了字符串"Donkey"是怎么被移除的：

	var fruits = ["Apple", "Orange", "Donkey"]

	alert("I remove "+fruits.pop()) //现在数组变成了：["Apple","Orange"]

	alert("Now length is: "+fruits.length) // "Donkey"已经被移除了
请注意pop()方法是如何改变数组自身的。
与pop()方法相对应的是push()方法，它可以向数组末端推入一项元素。

	var fruits = ["Apple", "Orange"]

	fruits.push("Peach"); // 现在数组为：["Apple", "Orange", "Peach"]

	alert("Last element is:"+fruits[fruits.length-1])
#### 练习
1.创建一个包含"Jazz"、"Blues"的数组style；
2.向数组推入字符串"Rock'n'Roll"；
3.将数组的倒数第二个值替换为“Classic”。数组应该变成["Jazz","Classic","Rock'n'Roll"]。该代码应该适用于任何数组的长度。
4.移出数组的最后一项并将其alert出来。

#### 答案

	// 1
	var styles = ["Jazz", "Bluez"]

	// 2
	styles.push("Rock'n'Roll") // or: styles[styles.length] = "Rock'n'Roll"

	// 3 
	styles[styles.length-2] = "Classic"

	// 4
	alert( styles.pop() )
## shift()方法和unshift()方法
pop()和push()方法操作数组的末端，你也可以用shift()方法移除数组的第一个值或者用unshift()方法在数组的前端添加一个值

	var fruits = ["Apple", "Orange"]
	 
	var apple = fruits.shift() // 现在数组为：["Orange"]
	 
	fruits.unshift("Lemon") // 现在：["Lemon", "Orange"]
	 
	alert(fruits.length) // 2
push()方法和unshift()方法都可以一次增加多个值：

	var fruits = ["Apple"]
	 
	fruits.push("Orange","Peach")
	fruits.unshift("Pineapple","Lemon") // 现在: ["Pineapple", "Lemon", "Apple", "Orange", "Peach"]
#### 练习
写一段代码，alert出数组arr的任意一项值：

	var arr = ["Plum","Orange","Donkey","Carrot","JavaScript"]
备注：获得从最小值min到最大值max（包括max）的代码是：

	var rand = min + Math.floor(Math.random()*(max+1-min))

#### 答案

	var arr = ["Plum","Orange","Donkey","Carrot","JavaScript"]
	 
	var rand = Math.floor(Math.random()*arr.length)
	 
	alert(arr[rand])
## 遍历数组
为了遍历所有元素，通常使用一个遍历所有索引的循环。
下面的例子展示了用for循环来遍历数组。

	var fruits = ["Pineapple", "Lemon", "Apple", "Orange", "Peach"]
	 
	for(var i=0; i<fruits.length; i++) {
	  alert(fruits[i])
	}
#### 练习
创建一个函数find(arr,value)，它能在给定数组查找某一个值，并返回这个值的索引，如果数组中没有这个值，则返回-1.例如：

	arr = [ "test", 2, 1.5, false ]
	 
	find(arr, "test") // 0
	find(arr, 2) // 1
	find(arr, 1.5) // 2
	 
	find(arr, 0) // -1

#### 答案
一个可行的方案可能如下：

	function find(array, value) {
	 
	  for(var i=0; i<array.length; i++) {
	    if (array[i] == value) return i;
	  }
	    
	  return -1;
	}
尽管这可能是错误的，因为使用==时0和false没有区别。
更正确的方式是使用===。并且，在新的ES5标准中，有原生的数组方法[indexOf](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf)。所以我们可以这样定义函数：

	function find(array, value) {
	  if (array.indexOf) return array.indexOf(value)
	 
	  for(var i=0; i<array.length; i++) {
	    if (array[i] === value) return i;
	  }
	    
	  return -1;
	}
	 
	var arr = ["a", -1, 2, "b"];
	 
	var index = find(arr, 2);
	 
	alert(index);
一个更聪明更进一步的方法，是通过检查indexOf方法是否存在来有条件地定义这个函数find，我们将在接下来的章节介绍这点。
#### 练习
创建一个函数filterNumeric(arr),它接收一个数组arr，返回一个新数组，新数组只包含数组arr中值为数字的项。
下例展示了这个函数应该如何工作：

	arr = ["a", 1, "b", 2];
	 
	arr = filterNumeric(arr); // 现在 arr = [1,2]
#### 答案
解决的办法是遍历数组，只有当值为数字将其添加到新数组。[点此查阅](http://javascript.info/play/tutorial/intro/array/filterNumeric.html)。
## join()方法和split()方法
有时候我们需要一个快速简洁的方法把数组转换成字符串。这正是join()方法的目的所在。
它用给定的分隔符将一个数组转换成字符串：

	var fruits = ["Lemon","Apple","Orange","Peach"];
	 
	var str = fruits.join(', ');
	 
	alert(str);
相反的操作用split()也很容易实现：

	var fruits = "Apple,Orange,Peach";
	 
	var arr = fruits.split(','); // arr 是 ["Apple", "Orange", "Peach"]
	 
	alert(arr[0]);
#### 练习
对象有一个className属性，这个属性值为用空格符分隔的类（class）名：

    var obj = {
      className: 'open menu'
    }
写一个函数addClass(obj,cls),当类名cls不存在时将会添加这个class：

	addClass(obj, 'new') // obj.className='open menu new'
	addClass(obj, 'open')  // 没有改变 (class已存在)
	addClass(obj, 'me') // obj.className='open menu new me'
	 
	alert(obj.className)  // "open menu new me"
备注：你的函数不应该增加多余的空格。
#### 答案
解决的办法是用split()方法分割className然后遍历。如果没有匹配的class，则添加这个class。循环能稍微优化性能：

	function addClass(elem, cls) {
	  for(var c = elem.className.split(' '), i=c.length-1; i>=0; i--) {
	    if (c[i] == cls) return
	  }
	     
	  elem.className += ' '+cls
	}
	 
	var obj = { className: 'open menu' }
	 
	addClass(obj, 'new')
	addClass(obj, 'open')
	alert(obj.className)   // open menu new
在上面的例子中，变量c定义在循环开始的位置，i被设置为它的最后一个索引。
循环本身是从后往前进行的，结束条件是i>=0。其原因是，i>=0校验比i更快。这避免了在变量c中查找length属性。
#### 练习
创建一个函数camelize(str)将字符串"my-short-string"转化"myShortString"。
因此，连字符后的所有部分都被驼峰代替。例如：

	camelize("background-color") == 'backgroundColor'
	camelize("list-style-image") == 'listStyleImage'
这样的函数在用CSS操作时可能是有用的。
注意。记住charAt()、substr()方法和检查能将字符串转换为大写的str.toUpperCase()函数。
#### 答案
有多种方法来实现这样的任务。
一个可能的解决方案[点此查阅](http://javascript.info/play/tutorial/intro/array/camelize.html)。
## 使用长度（length）修剪数组
可以使用长度属性如下修剪数组：

	var arr = [0, 1, 2, 3]
	 
	alert(arr[2]); // arr【2】存在
	 
	arr.length = 2; // 将数组长度改为2
	 
	alert(arr[2]); // arr【2】已被修剪出数组
你只要设置长度浏览器将会立即修剪数组。
## 数组是对象，因此...
事实上在JavaScript中的数组是内部的一种对象，不过扩展了自动长度和一些特殊方法。
这和某些语言中数组代表一个连续的内存段有所不同，和基于链表的队列/堆栈结构也有区别。
#### 数组的键为非数字
上面数组的键是数字，但键可以有任何名称：

	arr = []
	arr[0] = 5
	arr.prop = 10 // 不要这样做
但是不推荐这样做。数值型的数组适合于数字类的键，而对象用于关联键值对。通常没有理由将它们混合。

在JavaScript中，数组作为一个哈希（hash）表，表现出了一些的优点和缺点。

例如，push/pop方法只操作数组的最后一个元素，所以他们是极快的，可以说复杂度为O（1）。

我的意思是指，push只能在末端作用：

	var arr = ["My", "array"]
	arr.push("something")
	 
	alert(arr[1]) // string "array"

**shift/unshift方法速度很慢，因为他们必须对整个数组重新编号**。split方法也可能导致重新编号。
![0_1453818669304_shiftpush.png](/uploads/files/1453818589719-shiftpush.png) 
因此，利用shift/unshift通常比push/pop慢。数组越大，重新编排的工作就越多。
#### 问题
下面代码的结果是什么？为什么？

	arr = ["a", "b"]
	 
	arr.push( function() { alert(this) } )

	arr[arr.length-1]()  // ?
#### 答案
因为数组是对象，arr[arr.length-1]（）实际上是一个对象的方法调用，就像obj[method]（）。

	arr[arr.length-1]()
	 
	// 和arr[2]()一样
	 
	// 语法错误，但是等同于:arr.2()
	 
	// 重写为相同的样式obj.method()
在这种情况下,this = arr被传递给函数，所以arr的内容会被alert出来。

	arr = ["a", "b"]
	 
	arr.push( function() { alert(this) } )
	 
	arr[arr.length-1]() // "a","b",function
## 稀疏数组，length的细节
**在JavaScript的数组的length属性不是数组的长度，而是最后一个索引加1**。
在下面的例子中，我们向空的数组fruits添加了两个元素，但长度变为了100：

	var fruits = [] // 空数组
	 
	fruits[1] = 'Peach'
	fruits[99] = 'Apple'
	 
	alert(fruits.length)  // 100 (但是只有两个元素)
如果您尝试输出稀疏数组，浏览器的输出会跳过指标为空的值：

	var fruits = [] // 空数组
	 
	fruits[2] = 'Peach'
	fruits[5] = 'Apple'
	 
	alert(fruits)  // ,Peach,,,Apple (或者与这类似)
但是自然地，数组仅仅是一个具有两个键的对象。缺失的值不占空间。
当用数组方法操作时，稀疏数组表现怪异。它们不知道哪些索引被跳过了：

	var fruits = [ ]
	 
	fruits[1] = 'Peach'
	fruits[9] = 'Apple'
	 
	alert( fruits.pop() ) // pop出'Apple' (在索引9)
	alert( fruits.pop() )  // pop出undefined (在索引8)
尽量回避稀疏数组。无论如何，它的方法并不能很好的工作。可以使用对象来代替。
## 删除数组元素
我们知道，数组只是对象。因此，我们可以用delete删除一个值：

	var arr = ["Go", "to", "home"]
	 
	delete arr[1] // 现在 arr = ["Go", undefined, "home"]
	alert(arr[1]) // undefined
你看，这个值被删除了，但可能不是我们想要的样子，因为在数组内得到了一个undefined的孔。
delete运算符会删除键值对，这正是它所做的。当然，因为数组只是一个哈希（hash），因此那个槽变成了undefined。
更多的时候，我们需要删除一个元素而不留下索引之间的孔。这里还有另一种方法，该方法有助于做到这一点。
### splice()方法
splice()方法对JavaScript中的数组来说就像一把瑞士军刀，**它可以删除元素和替换它们**。
它的语法如下：
**arr.splice(index, deleteCount[, elem1, ..., elemN])**
从索引（index）开始删除指定数目（deleteCount）的元素，然后将元素1（elem1），...，元素N(elemN)粘贴到索引的位置。
让我们来看看几个例子。

	var arr = ["Go", "to", "home"]
	 
	arr.splice(1, 1)  // 从索引1的位置删除一个元素
	 
	alert( arr.join(',') ) // ["Go", "home"] (已经删除了一个元素)
这样，**你就可以使用splice()方法从一个数组中删除一个元素。数组中的数字会移动去填补空白**。

	var arr = ["Go", "to", "home"]
	 
	arr.splice(0, 1)  // 在索引1的位置删除一项元素
	 
	alert( arr[0] ) // "to"成为了第一项元素
下面的例子演示了如何替换元素。

	var arr = ["Go", "to", "home", "now"];
	 
	// 删除前3项元素然后增加两项
	arr.splice(0, 3, "Come", "here")
	 
	alert( arr ) // ["Come", "here", "now"]
splice()方法返回由被删除元素组成的数组：

	var arr = ["Go", "to", "home", "now"];
	 
	// 删除前两项元素
	var removed = arr.splice(0, 2)
	 
	alert( removed ) // "Go", "to" <-- 被删除元素组成的数组
splice()方法能够插入元素，只需设置删除数量为0。

	var arr = ["Go", "to", "home"];
	 
	// 从第二个位置删除0个元素，然后插入"my", "sweet"
	arr.splice(2, 0, "my", "sweet")
	 
	alert( arr) // "Go", "to", "my", "sweet", "home"
它也可以使用负的索引，这样将从数组的末端计数：

	var arr = [1, 2, 5]
	 
	//在元素索引-1位置(最后一个之前)，删除0个元素,然后插入3和4
	arr.splice(-1, 0, 3, 4)
	 
	alert(arr)  // 1,2,3,4,5
#### 练习
对象有一个className属性，这个属性值为用空格符分隔的类（class）名：

    var obj = {
      className: 'open menu'
    }
写一个函数removeClass(obj,cls),当类名cls存在时将会删除这个class：

	removeClass(obj, 'open') // obj.className='menu'
	removeClass(obj, 'blabla')  // 没有变化 (没有class去删除)
#### 答案
解决的办法是用split()方法分割className，然后遍历这个数组。如果有匹配的class，则从数组中删除这个class，最后再对这个数组使用join（）方法并返回。
我们会做一个稍微优化的方法：

	function removeClass(elem, cls) {
	  for(var c = elem.className.split(' '), i=c.length-1; i>=0; i--) {
	    if (c[i] == cls) c.splice(i,1)
	  }
	     
	  elem.className = c.join(' ')
	}
	 
	var obj = { className: 'open menu' }
	 
	removeClass(obj, 'open')
	removeClass(obj, 'blabla')
	alert(obj.className)   // menu
在上面的例子中，变量c定义在循环开始的位置，i被设置为它的最后一个索引。
循环本身是从后往前进行的，结束条件是i>=0。其原因是，i>=0校验比i更快。这避免了在变量c中查找length属性。
#### 练习
创建一个函数filterNumericInPlace(arr),它接收一个数组arr，然后将数组中的非数值类型的值删除。
下例展示了这个函数应该如何工作：

	arr = ["a", 1, "b", 2];
	 
	filterNumericInPlace(arr);
	 
	alert(arr)  // [1,2]
解决的办法是遍历数组，然后用arr.splice去删除非数值类型的值。[点此查阅](http://javascript.info/play/tutorial/intro/array/filterNumeric2.html)。
### slice()方法
您也可以使用slice（开始[，结束]）获取数组中的一部分：

	var arr = ["Why", "learn", "JavaScript"];
	 
	var arr2 = arr.slice(0,2) // 从索引0开始获得两个元素
	 
	alert(arr2.join(', ')) // "Why, learn"
注意，这种方法不会修改原数组，它只是复制它的一个切片（slice）。
可以省略第二个参数，这样可以获取从索引开始到数组末端的所有元素：

	var arr = ["Why", "learn", "JavaScript"];
	 
	var arr2 = arr.slice(1) // 从索引1开始获取所有元素
	 
	alert(arr2.join(', ')) // "learn, JavaScript"
该方法还支持负的索引，就像splice方法。
## reverse()方法
另一种有用的方法是reverse()方法。假设，我想要一个域名的最后一部分，比如从"my.site.com"中获得"com"。下面是我能做到这一点的一个例子：

	var domain = "my.site.com"
	 
	var last = domain.split('.').reverse()[0]
	 
	alert(last)
注意JavaScript是如何允许如reverse()[0]这样复杂的语法去调用一个方法，然后从得到的数组中获取一个元素。
其实，你可以撰写更长的调用，JavaScript语言的语法允许你这样做。
## 排序,sort(fn)方法
sort()方法，可以给数组排序：

	var arr = [ 1, 2, 15 ]
	 
	arr.sort()
	 
	alert( arr )   // 1, 15, 2
运行上面的例子。注意到了一些奇怪的事情吗？顺序是1，15，2。
这是因为sort()方法**将一切转换为字符串，并在默认情况下根据字典顺序排序**。
为了使这个方法更智能，我们需要传递自定义比较函数。它应该接受两个参数，并返回1，0或-1：

	function compare(a, b) {
	  if (a > b) return 1
	  else if (a < b) return -1
	  else return 0
	}
	 
	var arr = [ 1, 2, 15 ]
	 
	arr.sort(compare)
	 
	alert( arr )   // 1, 2, 15
现在，这个方法工作正确。
#### 练习
创建一个函数ageSort(people)按人的年龄来对数组排序：

	var john = { name: "John Smith", age: 23 }
	var mary = { name: "Mary Key", age: 18 }
	var bob = { name: "Bob-small", age: 6 }
	 
	var people = [ john, mary, bob ]
	 
	ageSort(people) // 现在people是[ bob, mary, john ]
输出排序后的人物名字。
#### 答案
该解决方案利用了数组的sort()方法和自定义比较函数：

	function ageCompare(a, b) {
	  if (a.age > b.age) return 1
	  else if (a.age < b.age) return -1
	  return 0
	}
	 
	function ageSort(people) {
	  people.sort(ageCompare)
	}
	 
	// 验证这个方法
	var john = { name: "John Smith", age: 23 }
	var mary = { name: "Mary Key", age: 18 }
	var bob = { name: "Bob-small", age: 6 }
	 
	var people = [ john, mary, bob ]
	 
	ageSort(people)
	 
	// 检查顺序
	for(var i=0; i<people.length; i++) {
	  alert(people[i].name)
	}
**更简短的方法**
下面的比较函数可能更短。这是另一种解决方案：

	people.sort(function(a,b) { return a.age - b.age })
这是有效的，因为并不是一定要返回1或-1或0，正数或者负数也可以。
## 关于数组定义的更多...
### new Array()
从技术上讲，有另一个语法来定义数组：

    var arr = Array("Apple", "Peach", "etc")

它很少使用，只是因为使用方括号[]更加简洁。
此外，在这里还有一个陷阱，因为使用new Array，当传入一个数字参数进行调用时会产生给定长度而值为undefined的数组：

    var arr = new Array(2,3) // 这是可以的，我们得到了 [2, 3]
 
    arr = new Array(2) // 我们得到了[2] ?
 
    alert(arr[0]) // 不！我们得到了数组[undefined, undefined]
上面例子输出了undefined，因为 new Array(number)创建一个空的数组，它的长度设置为数字number。
这可能是十分出人意料的。但是，如果你知道该功能，那么这里有一个很好的利用new Array(number)的例子：

    var indent = new Array(5).join('a') // aaaa (4次)
这是重复字符串的一个聪明办法。
### 多维数组
在JavaScript中，数组可以存储任何数据类型。

	var arr = ["My", "Small array", true, {name:'John'}, 345]
	alert(arr[1]) // Small array
这可以用于存储多维数组：

	var matrix = [
	  [1, 2, 3],
	  [4, 5, 6],
	  [7, 8, 9]
	]
	 
	alert(matrix[1][1]) // 中心元素
#### 练习
创建一个通用函数filter(arr,func)，用给定函数过滤数组。只有func(elem)返回true的元素才会组成结果。
每一个元素都会被传递给函数，返回的新数组只包含原数组中数字类型的值。
下面是一个它应该如何工作的例子：

	arr = ["a", -1, 2, "b"]
	 
	arr = filter(arr, isNumeric)
	// arr = [-1, 2], 结果只含数字类型的值
	 
	arr = filter(arr, function(val) { return val > 0 })
	// arr = [2] , 对于其他值函数会返回false
#### 答案
这个任务真的没有什么特别的。在JavaScript中传递和使用函数很容易。[点此查阅](http://javascript.info/play/tutorial/intro/array/filter.html)。
#### 练习
素数是这样一个自然数，它只有两个不同的自然数约数：1和它本身。

用厄拉多塞筛法(Eratosthenes sieve)去找寻给定整数n的所有小于或等于n的素数：
1.创建一个从2到n的连续整数组成的列表：（2，3，4，...，n）；
2.设p=2，为第一个素数；
3.从列表中删除所有小于或等于n的整数倍p的值。（2p，3p，4p，等）；
4.再设p为列表中未删除的第一个值；。
5.重复步骤3和4，直到p*p > n；
6.列表中的所有剩余的数字就是素数。

这里也有一个[动画](http://javascript.info/files/tutorial/intro/array/sieve.gif)

实现JavaScript中的厄拉多塞筛法。计算出100以内的所有素数的和，并alert出来。
#### 答案
答案是1060.
解决方法在http://javascript.info/play/tutorial/intro/array/sieve.html
## 小结
这是所有关于数组的深入介绍。
我们已经介绍了：
1.如何声明数组，这有两种语法
2.如何在数组两端添加、替换、删除数组
3.如何去遍历数组
4.如何将一个字符串分割成数组，并如何拼接回来
5.JavaScript中数组和对象的关系。
在95％的时间知道这些就足够了。欲了解更多的方法和示例，请参阅 [Array in Mozilla manual](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)