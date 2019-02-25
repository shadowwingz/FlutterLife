### 变量

#### 1. var

var 可以接受任何类型的变量，但是如果定义 var 变量的同时给它赋值，那么它的类型就确定下来了，无法再更改。比如下面这段代码是不行的。

```dart
var s = "Hello World";
s = 1;
```

运行上面的代码，会报错：

> A value of type 'dart.core::int' can't be assigned to a variable of type 'dart.core::String'

意思是，int 类型的数值无法赋值给 String 类型的变量。

s 变量在定义的时候就被赋值了，那么它的类型就确定下来了：`String` 类型，再给它赋值 int 类型，就会报错。

但是如果只是定义 var 变量不赋值，后面再赋值，那么它的类型是可以更改的。

```java
var s;
s = "Hello World";
print(s);
// 打印变量的运行时类型
print(s.runtimeType);
s = 1000;
print(s);
print(s.runtimeType);
s = 1.0;
print(s);
print(s.runtimeType);
```

输出：

```dart
Hello World
String
1000
int
1.0
double
```

可以看到，s 变量的类型是在不断变化的。


#### 2. dynamic 和 Object

var 类似，dynamic 和 Object 也可以接受任何类型的数据，但是不同的是，dynamic 和 Object 被赋值之后，可以更改类型。

```dart
dynamic s = "Hello World";
print(s);
print(s.runtimeType);
s = 1;
print(s);
print(s.runtimeType);
```

输出：

```dart
Hello World
String
1
int
```

dynamic 和 Object 虽然使用上没有什么区别，但仔细分析一下，还是有些区别的。

当我们调用 dynamic 类型的变量的任何方法时，都不会被警告，即使
这个方法并不存在（注意，不警告不代表不会报错）。而当我们调用 Object 类型的变量的方法时，只能调用 Object 类中的那几个方法，比如 `hashCode`、`toString()`、`runtimeType`，即使 Object 类型的变量实际指向的对象类型有别的方法可以调用。

```dart
class A {
  void test() {
    print('A test');
  }
}

Object obj = new A();
obj.test();
```

上面的代码中，我们定义一个新的类 A，在类里我们定义了一个 `test` 方法。然后我们创建了 Object 类型的对象，并把 A 对象赋值给 obj。

运行一下，会报错：

> Error: The method 'test' isn't defined for the class 'dart.core::Object'.
Try correcting the name to the name of an existing method, or defining a method named 'test'.

意思是，Object 中没有 test 方法。

```dart
class A {
  void test() {
    print('A test');
  }
}

dynamic d = new A();
d.test();
```

我们再换 `dynamic` 试一下，运行一下：

```dart
A test
```

可以看到，dynamic 是可以调用 `test` 方法的。

[What is the difference between dynamic and Object in dart?](https://stackoverflow.com/questions/31257735/what-is-the-difference-between-dynamic-and-object-in-dart/31295855)

#### 3. final 和 const

和 Java 中的 final 一样，Dart 中 final 修饰的变量只能被赋值一次。

const 和 final 类似，修饰的变量也只能赋值一次。但是 const 更加严格，以数组为例：

final ：

```dart
final array1 = [1,2,3];
array1[0] = 2;
print(array1);
```

输出：

```dart
[2, 2, 3]
```

const ：

```dart
const array2 = [1, 2, 3];
array2[0] = 2;
print(array2);
```

输出：

> Unsupported operation: Cannot modify an unmodifiable list

可以看到，final 修饰的数组，是可以更改数组内的元素。但是 const 修饰的数组，是不能更改数组内的元素。

另外，从上面的写法我们也可以看出，final 和 const 在使用的时候是可以不用写具体类型的，比如 String。

```dart
final String s1 = "";
final s2 = "";
const String s3 = "";
const s4 = "";
```

还有一点，const 变量是一个编译时常量，也就是说，编译时就要确定 const 变量的值，不能等到运行时才确定。Dart 中有一个 `DateTime.now()` 函数，可以返回当前时间，在不同时间调用这个函数会获取到不同的值。所以这个时间只有程序运行起来才能确定。

```dart
final str = DateTime.now();
const str2 = DateTime.now();
```

final 修饰的变量，是可以接收 `DateTime.now()` 的返回值。而 const 修饰的变量，会直接报错。

[Const, Static, Final, Oh my!](https://news.dartlang.org/2012/06/const-static-final-oh-my.html)

### 初始值

在 Java 中，`int` 类型变量如果没有初始化，那么这个变量会被初始化为 `0`，`boolean` 类型变量如果没有初始化，那么这个变量会被初始化为 `false`。但是在 Dart 中，无论是什么类型的变量，都会被初始化为 `null`，因为在 Dart 中，一切皆是对象。

### 可选参数（Optional parameters）

可选参数分为 `可选命名参数` 和 `可选位置参数`。

可选命名参数是指，调用函数的时候，可以指定 `参数名：参数值`。比如这样调用函数：

```dart
enableFlags(bold: true, hidden: false);
```

上面的代码中，`bold` 是参数名，`true` 是参数值。

对应的，函数的定义是这样的：

```dart
void enableFlags({bool bold, bool hidden}) {
  
}
```

在参数外层要加 `{}`，使用 `{参数1， 参数2}` 来指定命名参数。

我们可以发现，可选命名参数，比起普通的函数，看起来更加清晰。

可选位置参数是指，