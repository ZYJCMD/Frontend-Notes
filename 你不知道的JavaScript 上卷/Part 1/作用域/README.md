chapter 1 : 作用域

# 编译原理

## 1. 传统的编译原理
分三个步骤：
* 分词／词法分析(Tokenizing/Lexing)
* 解析/语法分析(Parsing)
* 代码生成

JS的引擎更复杂，在语法分析和代码生成有特定的步骤进行性能优化。
而且JS的编译过程不是发生在构建之前，而是执行前的几微秒。

## 2.作用域

* LHS: 变量出现在左侧时进行的查询。`赋值操作的目标查询`.
* RHS: 变量出现在右侧时进行的查询。`赋值操作的源头查询`. 准确来说是`非左侧`, 如`console.log(a)`.

```javascript
function foo(a) {
    console.log(a);
}
foo(2);
```
以上程序进行了 1 次 LHS， 2 次 RHS。

### 2.1 作用域嵌套

我觉得就是作用域链的概念： 在当前作用域无法找到某个变量时，引擎会在外层嵌套的作用域中继续查找，直到全局。

### 2.2 异常
对于LHS和RHS查询，分别会抛出不同的异常。
* 如果是RHS查询无法找到改变量，则是一个未声明的变量，引擎会抛出`ReferenceError`异常，如果调用错误的函数／属性，则会抛出`TypeError`异常。
* 如果是LHS，则有两种情况:
```javascript
1. 如果是宽松／慵懒模式，则全局作用域中会创造一个全局变量。
2. 如果是严格模式， 因为严格模式在行为上有很多不同，其中一个就是禁止自动或者隐式的创建全局变量，这时候也会抛出ReferenceError异常。
```
