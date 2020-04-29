
# Widget学习
### Center组件
```
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return Center(
      child: Text(
        "我是 Dart 一个文本内容",
        textDirection: TextDirection.ltr,
        style: TextStyle(
            fontSize: 40.0,
            fontWeight: FontWeight.bold, // color: Colors.yellow
            color: Color.fromRGBO(255, 222, 222, 0.5)),
      ),
    );
  }
}
```
### MaterialApp组件
MaterialApp 是一个方便的 Widget，它封装了应用程序实现 Material Design 所需要的 一些 Widget。一般作为顶层 widget 使用。

```
常用的属性： 
home（主页） 
title（标题） 
color（颜色） 
theme（主题） 
routes（路由）
```

### Scaffold组件
Scaffold 是 Material Design 布局结构的基本实现。此类提供了用于显示 drawer、 snackbar 和底部 sheet 的 API。

```
Scaffold 有下面几个主要属性： 
appBar - 显示在界面顶部的一个 AppBar。 
body - 当前界面所显示的主要内容 Widget。 
drawer - 抽屉菜单控件。
```
```
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return MaterialApp(
        title: "我是一个标题",
        home: Scaffold(
          appBar: AppBar(
            title: Text('Dart'),
            elevation: 30.0, //设置标题阴影 不需要的话值设置成 0.0 ),
            body: MyHome(),
          ),
          theme: ThemeData(
              //设置主题颜色
              primarySwatch: Colors.yellow),
        ));
  }
}
```
### Text 组件
| 名称 | 功能 |
| --- | --- |
| textAlign | 文本对齐方式（center 居中，left 左 对齐，right 右对齐，justfy 两端对齐） |
| textDirection | 文本方向（ltr 从左至右，rtl 从右至 左） |
| overflow | 文字超出屏幕之后的处理方式（clip 裁剪，fade 渐隐，ellipsis 省略号） |
| textScaleFactor | 字体显示倍率 |
| maxLines | 文字显示最大行数 |
| style | 字体的样式设置(TextStyle) |
下面是 TextStyle 的参数 ：

| 名称 | 功能 |
| --- | --- |
| decoration | 文字装饰线（none 没有线，lineThrough 删 除线，overline 上划线，underline 下划线） |
| decorationColor  | 文字装饰线颜色 |
| decorationStyle | 文字装饰线风格（[dashed,dotted]虚线， double 两根线，solid 一根实线，wavy 波浪 线） |
| wordSpacing | 单词间隙（如果是负值，会让单词变得更紧 凑 |
| letterSpacing | 字母间隙（如果是负值，会让字母变得更紧 凑） |
| fontStyle | 文字样式（italic 斜体，normal 正常体） |
| fontSize | fontSize 文字大小 |
| color | 文字颜色 |
| fontWeight | 字体粗细（bold 粗体，normal 正常体） |
更多参数：https://docs.flutter.io/flutter/painting/TextStyle-class.html

###  Container 组件

| 名称 | 功能 |
| --- | --- |
| alignment | topCenter：顶部居中对齐 topLeft：顶部左对齐 topRight：顶部右对齐 center：水平垂直居中对齐 centerLeft：垂直居中水平居左对齐 centerRight：垂直居中水平居右对齐 bottomCenter 底部居中对齐 bottomLeft：底部居左对齐 bottomRight：底部居右对齐 |
| decoration | decoration: BoxDecoration( color: Colors.blue, border: Border.all( color: Colors.red, width: 2.0, ),borderRadius: BorderRadius.all( Radius.circular(8.0) ) ) |
| margin | margin 属性是表示 Container 与外部其他 组件的距离。 EdgeInsets.all(20.0), |
| padding | padding 就 是 Container 的 内 边 距 ， 指 Container 边缘与 Child 之间的距离 padding: EdgeInsets.all(10.0) |
| transform | 让 Container 容易进行一些旋转之类的transform: Matrix4.rotationZ(0.2) |
| height | 容器高度 |
| width | 容器宽度 |
| child | 容器子元素 |
更多参数：https://api.flutter.dev/flutter/widgets/Container-class.html

### Image组件
图片组件是显示图像的组件，Image 组件有很多构造函数，这里是两个常见的 
Image.asset 本地图片
Image.network 远程图片

Image 组件的常用属性:

| 名称 | 类型 | 功能 |
| --- | --- | --- |
| alignment | Alignment | 图片的对齐方式 |
| color 和 colorBlendMode |  | 设置图片的背景颜色，通常和 colorBlendMode 配合一起 使用，这样可以是图片颜色和背景色混合。上面的图片就 是进行了颜色的混合，绿色背景和图片红色的混合 |
| fit | BoxFit | fit 属性用来控制图片的拉伸和挤压，这都是根据父容器来 的。BoxFit.fill:全图显示，图片会被拉伸，并充满父容器。 BoxFit.contain:全图显示，显示原比例，可能会有空隙。 BoxFit.cover：显示可能拉伸，可能裁切，充满（图片要 充满整个容器，还不变形）。BoxFit.fitWidth：宽度充满（横向充满），显示可能拉伸， 可能裁切。 BoxFit.fitHeight ：高度充满（竖向充满）,显示可能拉 伸，可能裁切。 BoxFit.scaleDown：效果和 contain 差不多，但是此属 性不允许显示超过源图片大小，可小不可大。| 
| repeat |   | ImageRepeat.repeat : 横向和纵向都进行重复，直到铺满整 个画布。 ImageRepeat.repeatX: 横向重复，纵向不重复。 ImageRepeat.repeatY：纵向重复，横向不重复。 |
| width | | 宽度 一般结合 ClipOval 才能看到效果 |
| height | | 高度 一般结合 ClipOval 才能看到效果 |
更多属性参考：https://api.flutter.dev/flutter/widgets/Image-class.html

```
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return Center(
      child: Container(
        child: Image.network(
          "http://pic.baike.soso.com/p/20130828/20130828161137-1346445960.jpg",
          alignment: Alignment.topLeft,
          color: Colors.red,
          colorBlendMode: BlendMode.colorDodge,
// repeat: ImageRepeat.repeatX,
          fit: BoxFit.cover,
        ),
        width: 300.0,
        height: 400.0,
        decoration: BoxDecoration(color: Colors.yellow),
      ),
    );
  }
}
```

引入本地图片
![图1](imageBag/2.png)
然后，打开 pubspec.yaml 声明一下添加的图片文件，注意要配置对
![图2](imageBag/3.png)

```
    return Center(
      child: Container(
        child: Image.asset("images/a.jpeg", fit: BoxFit.cover),
        width: 300.0,
        height: 300.0,
        decoration: BoxDecoration(color: Colors.yellow),
      ),
    );
```
图片圆角

```
    return Center(
      child: Container(
        width: 300.0,
        height: 300.0,
        decoration: BoxDecoration(
            color: Colors.yellow,
            borderRadius: BorderRadius.circular(150),
            image: DecorationImage(
                image: new NetworkImage(
                    "http://pic.baike.soso.com/p/20130828/20130828161137-1346445960.jpg"),
                fit: BoxFit.cover)),
      ),
    );
```
实现圆形图片
```
    return Center(
      child: Container(
          child: ClipOval(
        child: Image.network(
          "http://pic.baike.soso.com/p/20130828/20130828161137-1346445960.jpg",
          width: 150.0,
          height: 150.0,
        ),
      )),
    );
```

### Flutter列表组件概述
Flutter中可以通过ListView来定义列表项，支持垂直和水平方向展示。通过一个属性就可以控制列表的显示方向。列表有以下分类：
1、垂直列表
2、垂直图文列表
3、水平列表
4、动态列表
5、矩阵式列表

ListView列表参数

| 名称 | 类型  | 说明 |
| --- | --- | --- |
| scrollDirection | Axis | Axis.horizontal 水平列表 Axis.vertical 垂直列表 |
| padding | EdgeInsetsGeometry | 内边距 |
| resolve | bool | 组件反向排序 |
| children | List<Widget> | 列表元素 |


```
class HomeContent extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return Center(
      child: ListView(
        children: <Widget>[
          ListTile(
            leading: Icon(Icons.phone),
            title: Text(
              "this is list",
              style: TextStyle(fontSize: 28.0),
            ),
            subtitle: Text('this is list this is list'),
          ),
          ListTile(
            title: Text("this is list"),
            subtitle: Text('this is list this is list'),
            trailing: Icon(Icons.phone),
          ),
          ListTile(
            title: Text("this is list"),
            subtitle: Text('this is list this is list'),
          ),
          ListTile(
            title: Text("this is list"),
            subtitle: Text('this is list this is list'),
          ),
          ListTile(
            title: Text("this is list"),
            subtitle: Text('this is list this is list'),
          )
        ],
      ),
    );
  }
}
```
水平列表

```
class HomeContent extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return Container(
        height: 200.0,
        margin: EdgeInsets.all(5),
        child: ListView(
          scrollDirection: Axis.horizontal,
          children: <Widget>[
            Container(
              width: 180.0,
              color: Colors.lightBlue,
            ),
            Container(
              width: 180.0,
              color: Colors.amber,
              child: ListView(
                children: <Widget>[
                  Image.network(
                      'https://resources.ninghao.org/images/childhood-in-a-picture.jpg'),
                  SizedBox(height: 16.0),
                  Text(
                    '这是一个文本信息',
                    textAlign: TextAlign.center,
                    style: TextStyle(fontSize: 16.0),
                  )
                ],
              ),
            ),
            Container(
              width: 180.0,
              color: Colors.deepOrange,
            ),
            Container(
              width: 180.0,
              color: Colors.deepPurpleAccent,
            ),
          ],
        ));
  }
}
```
动态ListView

```
class HomeContent extends StatelessWidget {
  List list = new List();
  HomeContent({Key key}) : super(key: key) {
    for (var i = 0; i < 20; i++) {
      list.add("这是第${i}条数据");
    }
    print(list);
  }
  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return ListView.builder(
      itemCount: this.list.length,
      itemBuilder: (context, index) {
        // print(context);
        return ListTile(
          leading: Icon(Icons.phone),
          title: Text("${list[index]}"),
        );
      },
    );
  }
}
```
### GridView 组件
GridView 创建网格列表有多种方式，下面是主要的两种。
1、可以通过 GridView.count 实现网格布局 
2、通过 GridView.builder 实现网格布局
常用属性：

| 名称 | 类型  | 说明 |
| --- | --- | --- |
|scrollDirection| Axis| 滚动方法|
| padding| EdgeInsetsGeometry| 内边距|
|resolve |bool |组件反向排序 |
|crossAxisSpacing |double |水平子 Widget 之间间距 |
| mainAxisSpacing| double| 垂直子 Widget 之间间距 |
|crossAxisCount| int |一行的 Widget 数量|
| childAspectRatio| double| 子 Widget 宽高比例 |
|children| | \<Widget\>[] | 
|gridDelegate| SliverGridDelegateWithFix edCrossAxisCount（常用） SliverGridDelegateWithMax CrossAxisExtent| 控制布局主要用在 GridView.builder 里面|
GridView.count 实现网格布局

```
class LayoutContent extends StatelessWidget {
  List<Widget> _getListData() {
    var tempList = listData.map((value) {
      return Container(
        child: Column(
          children: <Widget>[
            Image.network(value["imageUrl"]),
            SizedBox(height: 12),
            Text(value["title"],
                textAlign: TextAlign.center, style: TextStyle(fontSize: 20)),
          ],
        ),
        decoration: BoxDecoration(
            border: Border.all(
                color: Color.fromRGBO(230, 230, 230, 0.9), width: 1.0)),
      );
    }); // ('124124','124214')
    return tempList.toList();
  }

  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return GridView.count(
      padding: EdgeInsets.all(20), crossAxisCount: 2, crossAxisSpacing: 20,
      mainAxisSpacing: 20, // childAspectRatio:0.7,
      children: this._getListData(),
    );
  }
}
```
GridView.builder 实现网格布局

```
class LayoutContent extends StatelessWidget {
  Widget _getListData(context, index) {
    return Container(
      child: Column(
        children: <Widget>[
          Image.network(listData[index]["imageUrl"]),
          SizedBox(height: 12),
          Text(listData[index]["title"],
              textAlign: TextAlign.center, style: TextStyle(fontSize: 20)),
        ],
      ),
      decoration: BoxDecoration(
          border: Border.all(
              color: Color.fromRGBO(230, 230, 230, 0.9), width: 1.0)),
    );
  }

  @override
  Widget build(BuildContext context) {
    // TODO: implement build

    return GridView.builder(
      itemCount: listData.length,
      gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
          //横轴元素个数
          crossAxisCount: 2, //纵轴间距
          mainAxisSpacing: 20.0, //横轴间距
          crossAxisSpacing: 10.0, //子组件宽高长度比例
          childAspectRatio: 1.0),
      itemBuilder: this._getListData,
    );
  }
}

```
