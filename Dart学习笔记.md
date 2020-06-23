
# Dart学习
## 第二章 对象、接口、类与mixin
### Mixin（关键字with）
Mixin的作用是为了继承接口
引用一个很好的解释
[形象说明](https://kevinwu.cn/p/ae2ce64/#Java%E7%89%88%E6%9C%AC%E5%AE%9E%E7%8E%B0)
##### 需要注意的点
1.`class X with Y`这样写则相当于继承自Object 然后 实现Y类的接口即
`class X extends Object with Y`
2.`X with Y` 若Y是抽象类，且没实现所有的接口，则X需要实现Y未实现的接口。
3.


## 第三章 库
### 库的导入
##### 1.完全导入
假定需要在A库里使用B库的某些方法，我们需要将B库import进A库。
```
import 'B.dart';
```
不仅如此，dart的导入语句还适用于任意url。我们可以使用网络上的某个资源（dart文件）作为外部库导入。
```
import 'http://hello/hello.dart';
```
但是我们不推荐这样做。因为网络资源随时可能会发生变化。一旦改变，我们的程序将会被破坏。
真正严谨的做法是：
```
import'package:hello/hello.dart';
```
使用 package：导入方式会执行一个常驻的封装了代码位置信息的包管理器。
一个库可以使用的全部对象包括这个库本身声明的，以及通过导入语句从其他库导入的。在dart：core中定义的对象是隐式导入的。而一个库对外可使用的对象称为库导出的命名空间。
理论上一个库的命名空间中不应有名称相同的两个对象，否则你需要使用别名。
```
import 'test2.dart'
class Test{
    static final hello = new Test();
}
```

```
class Test2{
    static final hello = new Test2();
    Test2 _test = new Test2();
}
```
在这段代码中，Test2被import进了Test库，我们在test库中是无法看到Test2的私有变量_test的，所以这个变量将不会被导入到命名空间。
在Dart中，当前库所声明的对象优先级高于任何对象，因此导入的库中有顶层对象并不会有想象中那样具有破坏性。但是如果你访问了一个导入的对象，另外一个导入后续又添加了一个同名对象，那么新导入的对象会覆盖原有对象。

##### 2.不完全导入
Dart提供了额外的机制来控制导入到库内的对象：命名组合器show和hide。

`show`
当我们只需要一个庞大库中某一个或某几个（少数）的对象的时候，我们可以选择使用show组合器进行导入。这样可以使你的库更加健壮。
```
import 'package:math' show Random;
```
在这行代码中，我们只导入了math库中的Random对象。
show组合器接收一个命名空间和一个标识符列表，并将标识符列表中出现的对象保留在命名空间。

`hide`
当我们在一个库种希望不导入某一个或某几个对象的时候，我们可以使用hide组合器进行导入。
```
import 'package:math' hide Random;
```
这段代码将导入math库但不导入math库种的Random对象。实现方式与show类似。同样也是接收一个命名空间和标识符列表，并将标识符列表中出现的对象从命名空间中丢弃，然后产生一个新的命名空间。

##### 3.解决变量名冲突的办法
解决此问题最好的办法是将引入的库加上别名。
```
import 'package:math' as mymath;
```
通过这种方式我们可以完美避开不同库之间因为导入而使得变量名冲突的问题。

### 库的拆分
有的时候一个库可能太大，不能方便的保存在一个文件当中。Dart允许我们把一个库拆分成一个或者多个较小的part组件。或者我们想让某一些库共享它们的私有对象的时候，我们需要使用part。

下面为两个不同文件的代码片段
```
//文件为 part1.dart
import 'package:flutter/material.dart';
part 'part1.dart';
```

```
//文件为part2.dart
part of 'part1.dart';
class MyWidget extends StatelessWidget {
    @override 
    Widget build(BuildContext context) {
        ...
    }
}
```
这里我们可以看到，part2.dart是part of 'part1.dart'的文件，可以理解为，part2是part1的一部分。在part2.dart中，我们并没有引入package:flutter/material.dart包就直接继承了StatelessWidget，是因为，在part中，import进来的库是共享命名空间的。

不是所有的库都有名称，但如果使用part来构建库，那么库必须要命名。
```
library xxx;
```
每个子part都存放在各自的文件中。但是它们共享同一作用域，库的内部命名空间，以及所有的导入（import）。

##### Part与import的区别
1.可见性：
如果说在A库中import了B库，A库对B库是不可见的，也就是说B库是无法知道A库的存在的。而part的作用是将一个库拆分成较小的组件。两个或多个part共同构成了一个库，它们彼此之间是知道互相的存在的。

2.作用域：import不会完全共享作用域，而part之间是完全共享的。如果说在A库中import了B库，B库import了C库，A库是没有办法直接使用C库的对象的。而B,C若是A的part，那么三者共享所有对象。并且包含所有导入。

## 第四章 函数
#### 参数
###### 1.位置参数可以使必填或可选
```
add(a, b) => a + b;//必填
```
```
add(a, [b = 0]) => a + b;//可选
```
###### 2.命名参数
命名参数必须放在位置参数之后声明并用大括号包裹
```
add({int a, int b}) => a + b;
```
命名参数都是可选的，上面的函数可以
`add(a:1,b:2);`直接调用，但是`add()`这样调用虽不会触发编译错误，在运行时则会抛出异常`Unhandled Exception: NoSuchMethodError: The method '+' was called on null.`


## 第五章 类型
1.Dart中的变量都是对象，所以
`int a;`这种变量默认值是null，
注意书写规范


## 第八章 异步
`stream：一个长度不确定的值列表。可能是无限的，或者最终可能结束，关键是不知道stream何时结束或者是否已经结束。
eg. 随时间而改变的鼠标位置、所有素数的列表、通过网络发送给我们的市况视频。
`

`isolate：Dart支持actor风格的并发模型。运行中的Dart程序由一个或多个actor组成，他们被称为isolate，它有着自己的内存和单线程控制的计算过程。因为Dart没有共享内存的并发，所以不需要锁而且不会发生竞争。由于isolate没有共享内存，所以他们之间通信的唯一方式是通过消息传递，Dart中消息传递都是异步的。isolate没有阻塞接收机制，因此不会有死锁`

`Port：一个isolate有多个port，他是isolate间通信的底层基础，他有 
1.receive port（收消息的stream）
2.send port（允许发送消息给isolate，可被receive port生成，它将把所有消息发送给对应的receive port）`

`spawning：在isolate中启动另一个isolate被称为spawning。（新生成的isolate成为spawnee，生成spawnee的isolate是spawner。）`

##### 异步函数
async await
```
Future<String> foo() async {
  print('main 2');
  String value = await bar();
  print('main 5 $value');
  return "finish";
}

mainxx() {
  print('main 1');
  final f = foo();
  print("main $f");
  f.then((value) {
    print("main then $value");
  });
  print("main 4");
}

bar() {
  print("main 3");
  return "result";
}

void main() {
  mainxx();
}
```
调用结果
```
flutter: main 1
flutter: main 2
flutter: main 3
flutter: main Instance of 'Future<String>'
flutter: main 4
flutter: main 5 result
flutter: main then finish
```
##### 异步Generator
1.sync\* （同步生成器：返回一个Iterable对象）
被「sync*」标记的函数，一定要返回一个 「Iterable」，这样的函数生成器叫做同步生成器：
```
Iterable <int> foo2() sync*{
  print('foo2 start');	
  for(int i = 0; i < 3; i++) {
    print('运行了foo2，当前index：${i}');
    yield i;	
  }	
  print('foo2 stop');	
}
```
直接调用函数foo2并不会执行
当我们调用 foo2()的时候，这里会马上返回一个 Iterable，就像网络请求会马上返回一个 Feature一样。
但是在我们没有调用 Iterable 的 moveNext 的时候，当前函数体是不会执行的。
而当我们调用了 moveNext 方法后，代码会执行到 yield 关键字的位置，并且在这里停住。
当我们再一次调用 moveNext 后，会再恢复执行，然后再次停到 yield 关键字的位置，依次循环，当没有下一个值得时候，函数会隐式的调用 return方法来终止函数。
```
var b = foo2().iterator;	
print('还没开始调用 moveNext');	
b.moveNext();	
print('第${b.current}次moveNext');	
b.moveNext();	
print('第${b.current}次moveNext');	
b.moveNext();	
print('第${b.current}次moveNext');
```
结果
```
还没开始调用 moveNext	
foo2 start	
运行了foo2，当前index：0	
第0次moveNext	
运行了foo2，当前index：1	
第1次moveNext	
运行了foo2，当前index：2	
第2次moveNext
```
2.async\* 异步生成器：返回Stream对象
常用的是async
现在有一个这样的需求，我想每隔一秒钟请求一下数据，一共请求10次，看看有没有人关注我等等，
如果使用原始的 async
```
getData() async {	
  for (int i = 0; i < 10; i++) {
    await Future.delayed(Duration(seconds: 1), ()async {	
        Data data = await getXXX();
      setState(){	
        //业务逻辑	
      };	
    });	
  }	
}
```
这里使用循环，然后每一秒钟请求依次接口，返回数据后 setState();
这样肯定不行，因为你不可能一两秒钟就 setState()一次，
这个时候 async\* 就派上用场了：
```
Stream<Data> getData() async* {
  for (int i = 0; i < 10; i++) {
    await Future.delayed(Duration(seconds: 1));	
    yield await getXXX();	
  }	
}
```
在页面上，我们可以用 StreamBuilder 来包住，这样每次返回数据就不用 setState() 了。

```
void main() {
  var n = getData();
  // print("n = $n");
  var p = -1;
  final stream = StreamBuilder(
    stream: n,
    initialData: null,
    builder: (BuildContext context, AsyncSnapshot snapshot) {
      print("data = ${snapshot.data}");
      return Container(
        child: null,
      );
    },
  );
  runApp(stream);
}
Stream<int> getData() async* {
  for (int i = 0; i < 3; i++) {
    await Future.delayed(Duration(seconds: 1));	
    yield await i;	
    // yield await getXXX(i);	
  }	
}
getXXX(int i) {
  return i;
}
```
执行结果 关键在于yield修饰语句
```
flutter: data = null
flutter: data = 0
flutter: data = 1
flutter: data = 2
```

3.await-for循环
如果在实现异步 for 循环时遇到编译时错误，请检查确保 await for 处于异步函数中。 例如，要在应用程序的 main() 函数中使用异步 for 循环，main() 函数体必须标记为 async：

```
Future main() async {
  // ...
  await for (var request in requestServer) {
    handleRequest(request);
  }
  // ...
}
```
                    学习心得
        高尔基曾说过：“书是人类的进步阶梯”，我非常认同这个观点，无巧不成书，我们研发中心购入了《Dart变成语言》这本书供我们研发人员学习，这对我这个正在研究Flutter跨平台开发的开发人员而言无疑是暗室逢灯。
        看了第一张简介你就能发现，Dart是一本纯面向对象的语言，并无基本类型一说，习惯了使用基本类型的开发人员需要一点适应过程。关注的行为还是你行为而非内部实现。
        Dart的类即接口，extends提供常规继承，implements则是继承接口声明，mixin可以满足类似于多继承的功能，这不禁让我觉得Dart不愧为一门集大家之所长的高级编程语言。
         库总是为了模块化而存在，分part则是为了拆分大型库。
        参数、语句、表达式这些都跟常规高级编程语言基本一致，区别点是主要是命名参数必须是可选的。
        由于反射mirrors.dart库官方目前还是承认其不稳定，开发者经常被建议避开他，所以不做深入研究，其基本作用是，获取类的内部成员（变量、方法）。
        多线程方面的重点还是await、async、Future等。
        本次学习，收获还是很多，但这不是全部，毕竟书读百遍，其义自见，也许过了半个月再次翻阅时又会有更深一层面的理解。

