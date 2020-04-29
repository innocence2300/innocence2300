```
function server(params){
	new Promise((res,rej)=>{.then(res){}.catch(err)}{})
}
```

==Promise解决回调地狱问题
Promise中实现了两个保存成功
结果与失败结果的数组==
开源论坛多逛逛
##### **变量提升**
1. JS的数据类型(Number，String，Boolean，Null，undefined，Symbol,bigint)
2. 引用数据类型(Object,function)function的原型链直接指向function
3. object又可以细分为：
	普通对象
	数组对象
	正则对象
	日期对象
	Math数学函数对象
	...
4. 数据类型检测：
```
typeof:检测数据类型的逻辑运算符
instanceof：检测是否为某个类的实例
constructor：检测构造函数
Object.prototype.toString.call(8，-1):用于检测数据类型
```
```
Array.isArray:检测是否为数组
typeof[value] 返回当前值的数据类型 “数据类型”
typeof null的检测结果是object，但是null是基本数据类型
主要由于所有值在内存中都是按照二进制存储的，而null以二进制存储的值
是与检测出“object”的结果值是相同的
typeof function(){}的检测结果是”function“
```

typeof的返回结果都是字符串，且typeof不能细分对象类型（检测普通对象与数组对象都是“object”）

```
Number详解：NaN/isNaN/infinity/parseInt
NaN!=NaN 它和谁都不相等
把其他数据类型转换为数字有哪些方法：
强转换（基于底层机制进行转换）Number([value])
一些隐式装换都是基于Number完成的
isNaN('12px')先把其他类型转换为数字再进行检测
数学运算 '12px-13'
字符串==数字 两个等号比较很多时候会把其他转换为数字再进行比较
```
```
parseInt("")  Number("") isNaN("") parseInt(null)
Number(null) isNaN(null)  parseInt('12px') Number('12px')
Number('12px')  isNaN('12px')  parseFloat("1.6px") + parseInt("1.2px")+typeof parseInt(null)
isNaN(Number(!!Number(parseInt("0.8"))))
typeof !parseInt(null)+!isNaN(null)
```
弱转换（基于一些额外的方法）```
parseInt(),parseFloat()
```