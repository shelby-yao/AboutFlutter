Flutter架构学习-MVVM
MVVM架构依赖于状态管理框架，此处以Provider为例，这里封装一个widget，如下
```
class ProviderWidget<T extends BaseViewModel> extends StatefulWidget {
  ProviderWidget({Key key, this.model, this.child, this.onReady, this.builder})
      : super(key: key);

  final Widget Function(BuildContext context, T value, Widget child) builder;
  final T model;
  final Widget child;
  final Function(T) onReady;

  @override
  _ProviderWidgetState<T> createState() => _ProviderWidgetState<T>();
}

class _ProviderWidgetState<T extends BaseViewModel> extends State<ProviderWidget<T>> {
  @override
  void initState() {
    // TODO: implement initState
    super.initState();
    if (widget.onReady != null) {
      widget.onReady(widget.model);
    }
  }

  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider<T>(
      create: (_) => widget.model,
      child: Consumer<T>(
        builder: widget.builder,
        child: widget.child,
      ),
    );
  }
}



class ProviderWidget2<A extends BaseViewModel, B extends BaseViewModel>
    extends StatefulWidget {
  final Widget Function(BuildContext context, A model1, B model2, Widget child)
  builder;
  final A model1;
  final B model2;
  final Widget child;
  final Function(A, B) onModelReady;

  ProviderWidget2({
    Key key,
    @required this.builder,
    @required this.model1,
    @required this.model2,
    this.child,
    this.onModelReady,
  }) : super(key: key);

  _ProviderWidgetState2<A, B> createState() => _ProviderWidgetState2<A, B>();
}

class _ProviderWidgetState2<A extends BaseViewModel, B extends BaseViewModel>
    extends State<ProviderWidget2<A, B>> {
  A model1;
  B model2;

  @override
  void initState() {
    model1 = widget.model1;
    model2 = widget.model2;

    if (widget.onModelReady != null) {
      widget.onModelReady(model1, model2);
    }

    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    return MultiProvider(
        providers: [
          ChangeNotifierProvider<A>(
            create: (context) => model1,
          ),
          ChangeNotifierProvider<B>(
            create: (context) => model2,
          )
        ],
        child: Consumer2<A, B>(
          builder: widget.builder,
          child: widget.child,
        ));
  }
}
```
其中BaseViewModel为viewmodel基类，继承于ChangeNotifier,其最基础的作用是定义几种状态(如init loading emtpy finish等),并在状态改变的时候调用notifyListeners()更新UI,网络请求结果msg可以直接用viewmodel去完成toast,page要做的是根据viewmodel的状态（init loading empty finish等）去展现不同的状态UI,onModelReady闭包为的是实现某些页面第一次加载时需要进行初次网络请求的需求,这层封装在MVVM架构下尤其重要。


```
class BaseViewModel extends ChangeNotifier {
  BaseState state = BaseState.INITIAL;
  String failReaseon;
  bool autoToast = false;

  update() {
    notifyListeners();
  }

  changeState(BaseState newState, {String reason}) {
    state = newState;
    if (newState != BaseState.FAIL) {
      failReaseon = null;
    } else {
      failReaseon = reason;
      if (autoToast) {
        Jtoast.showToast(failReaseon);
      }
    }
    notifyListeners();
  }
}
```
上面是最简易的viewmodel基类，项目结构颗粒更细的时候可以发展子类如ListViewModel(增加dataList\<T>属性 以及 Future\<List\<T>> loadData()空实现方法/抽象方法等,并且可以将下拉刷新控件的controller交给viewmodel进行UI刷新状态管理)
eg.  ListViewModel继承于BaseViewModel继承于ChangeNotifier

```
abstract class ViewStateRefreshListModel<T> extends ListViewModel<T> {
  /// 分页第一页页码，具体项目可能是0 或者 1作为第一页
  static const int pageNumFirst = 0;

  /// 分页条目数量
  static const int pageSize = 20;
//此处用的是pull_to_refresh下拉刷新库
  RefreshController _refreshController =
      RefreshController(initialRefresh: false);

  RefreshController get refreshController => _refreshController;

  /// 当前页码
  int _currentPageNum = pageNumFirst;

  /// 下拉刷新
  ///
  /// [init] 是否是第一次加载
  /// true:  Error时,需要跳转页面
  /// false: Error时,不需要跳转页面,直接给出提示
  Future<List<T>> refresh({bool init = false}) async {
    try {
      _currentPageNum = pageNumFirst;
      var data = await loadData(pageNum: pageNumFirst);
      if (data.isEmpty) {
        refreshController.refreshCompleted(resetFooterState: true);
        list.clear();
        setEmpty();
      } else {
        onCompleted(data);
        list.clear();
        list.addAll(data);
        refreshController.refreshCompleted();
        // 小于分页的数量,禁止上拉加载更多
        if (data.length < pageSize) {
          refreshController.loadNoData();
        } else {
          //防止上次上拉加载更多失败,需要重置状态
          refreshController.loadComplete();
        }
        setIdle();
      }
      return data;
    } catch (e, s) {
      /// 页面已经加载了数据,如果刷新报错,不应该直接跳转错误页面
      /// 而是显示之前的页面数据.给出错误提示
      if (init) list.clear();
      refreshController.refreshFailed();
      setError(e, s);
      return null;
    }
  }

  /// 上拉加载更多
  Future<List<T>> loadMore() async {
    try {
      var data = await loadData(pageNum: ++_currentPageNum);
      if (data.isEmpty) {
        _currentPageNum--;
        refreshController.loadNoData();
      } else {
        onCompleted(data);
        list.addAll(data);
        if (data.length < pageSize) {
          refreshController.loadNoData();
        } else {
          refreshController.loadComplete();
        }
        notifyListeners();
      }
      return data;
    } catch (e, s) {
      _currentPageNum--;
      refreshController.loadFailed();
      debugPrint('error--->\n' + e.toString());
      debugPrint('statck--->\n' + s.toString());
      return null;
    }
  }

  // 加载数据
  Future<List<T>> loadData({int pageNum});

  @override
  void dispose() {
    _refreshController.dispose();
    super.dispose();
  }
}
```
  Future<List<T>> loadData({int pageNum});加载数据方法交给真正的具体子类去实现


Flutter中MVVM架构思路以及简单实现
的一个解耦问题尚未找到合理的解决方案，其问题如下

mvvm中
model->即瘦model，仅提供转序列化 反序列化方法，
位于架构底层，即拿即用。
viewModel->负责发起网络请求，保存对应数据及状态，根据不同状态提示提醒视图更新UI，vm与视图中的连接目前以Provider实现，这是一个业务类，具体的实现类（非基类vm）定制性很强，基本上不能复用，优秀的基类vm可以项目通用。
view->视图，flutter中没有控制器的概念，只要是视图都是widget子类，它用于展现内容以及提供交互
这里我们能够发现model是属于最底层的模块，他不依赖于其他模块，vm则会在请求中获取model，即vm持有model，另外vm可能会具有一些提示相关功能（toast等），所以他会引入一些三方库/基于三方库的二次封装库。

这里还有疑惑的问题是view的耦合性，理论上view也是仅仅负责UI展示的类，也是即拿即用的类型，新项目有相似UI可以直接拿老项目的view直接用，可是由于Provider的消费者获取方法（这里xvm不能以协议的方式去拿，只能真实vm类的方式去拿）
```
Provider.of<xvm>(context)
```
即view中获取vm即时状态时要拿到vm,此刻view就引用了vm以及m，从而导致view被耦合，无法被新项目即拿即用。
我查看了一些优秀的Flutter mvvm开源项目（例如fun flutter），发现目前这种情况并未解决。一时之间还难以想出解决方案。