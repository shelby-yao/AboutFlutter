# LaunchScreen启动图上架设置
1. info.plist 删除 Application Scene Manifest
2. 删除SceneDelegate.swift
3. 删除AppDelegate.swift中UISceneSession Lifecycle相关方法
4. 拖一个ImageView进入LaunchScreen.storyboard
5. 约束该ImageView上下左右顶死父视图（不留安全区，完全顶死）
6. 导入一张png图片，放入主项目工程目录，将ImageView的图片设置为它（该图片要求设计好，避免不同机型剪切后产生显示不完全或者拉伸）
7. info.plist 删除 Main storyboard file base name AppDelegate.swift设置以自建控制器启动并按需添加启动图展示时间
8. 如果需要完全适配各尺寸屏幕，可以选择多Image相对布局的方式载入2x,3x图片
```
class AppDelegate: UIResponder, UIApplicationDelegate {
var window: UIWindow?
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        Thread.sleep(forTimeInterval: 3)
        self.window = UIWindow()
        self.window?.makeKeyAndVisible()
        self.window?.backgroundColor = .white
        self.window?.rootViewController = UINavigationController(rootViewController: ViewController());
        return true
    }
}
```