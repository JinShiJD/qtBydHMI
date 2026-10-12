# QT_BYD_HMI   
基于 Qt/QML 与 C++ 的比亚迪车载智能座舱 HMI 系统  
平台工具：Qt 6.8.3、Qt Creator、CMake/qmake 双构建系统、QML/Qt Quick  
项目内容：  
1、采用 C++ 单例模式 (Q_GLOBAL_STATIC 宏) 实现全局状态管理类 Interface ,统一管理页面路由、空调温度、设置参数、控制中心开关等跨页面共享状态,通过 #define INTERFACE (Interface::instance()) 宏简化全局调用  
2、基于 Qt 元对象系统 实现 C++ 与 QML 的双向数据绑定：C++ 端通过 Q_PROPERTY 暴露属性(含 READ/WRITE/NOTIFY), QML 端直接读写属性触发 C++ setter并发射 signal 通知界面更新;通过 QQmlContext::setContextProperty() 将 C++ 对象注册到 QML 上下文, 实现跨语言层无缝通信  
3、设计 Loader 动态页面路由机制 : 在 Main.qml 中使用 Loader + switch-case 路由表实现 12 个页面的按需加载与切换, setPageIndex() 自动记录 previousPageIndex 支持返回键导航，页面切换零开销（未激活页面不占用内存）  
4、构建分层组件化架构 (三层): components/ 原子组件层(20+ 个可复用控件, 如 IconButton 、 IconSwitch 、ColorSlider 、SwipeArea手势识别等) -> modules/ 独立功能模块层(仪表盘子系统)->screens/ 完整页面层（12 个功能页面），组件间通过 signal 通信，遵循高内聚低耦合原则  
5、实现 仪表盘仿真模块 : SpeedDial (速度表)、RpmDial(转速表)、FuelMeter(油量表) 三个独立表盘组件, 指针旋转采用 SpringAnimation 弹簧物理动画（spring=1.4, damping=0.15），模拟真实机械表盘的弹性阻尼效果  
6、实现 空调双温区控制 : 自定义 QuickTemperatureList 垂直温度选择器（16°C-32°C），当前选中项高亮放大，相邻项按距离递减字号与透明度(opacity 1→0.6→0.5→0.3), 支持惯性滚动(flickDeceleration) 与 movementEnded 回调  
7、实现 空调双温区控制 ：自定义 QuickTemperatureList 垂直温度选择器（16°C-32°C），当前选中项高亮放大，相邻项按距离递减字号与透明度（opacity 1→0.6→0.5→0.3），支持惯性滚动（ flickDeceleration ）与 movementEnded 回调  

## 部分界面展示  

![menu](./show_result/menu.png)

![air-conditioning](./show_result/air-conditioning.png)

![navigation](./show_result/navigation.png)

![split](./show_result/split.png)
