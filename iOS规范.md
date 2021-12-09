# iOS编码规范

1. 【强制】如果是响应事件类型的Delegate，Delegate命名统一规范：类名+Delegate；例如：UITableView + Delegate  或者  UITextField + Delegate
2. 【强制】第一个参数为本类实例对象，这么做的好处是清晰的知道该代理方法属于哪个对象的代理方法;
3. 【强制】代理方法中不要使用set和get开头，set和get在iOS里有特殊的含义
4. 【强制】禁止#define定义常量；宏定义只是在预编译阶段进行文本替换，不进行类型检查，大量的宏定义常量会影响编译速度
5. 【强制】根据常量的作用域，正确定义常量的方式：全局常量（extern const）和静态常量（static const）；
   * 如果只是在某个类内部使用，请使用static const定义常量；
   * 如果常量要被其他类使用，请使用extern const来定义外部常量，在.h中声明常量，在.m中定义
6. 【强制】实例变量具有下划线。命名遵循驼峰式规则
7. 【强制】属性名遵循驼峰式，禁止使用下划线命名属性名，UI控件必须遵循UI控件命名规范，Founction属性必须遵循Founction对象命名规范
8. 【强制】使用typedef NS_ENUM来定义枚举，禁止使用enum来定义枚举
9. 【强制】在枚举后进行注释，说明该枚举值的含义
10. 【强制】请使用NS_OPTIONS定义一组相互关联的位移枚举常量。位移枚举常量是可以组合使用的。枚举项以枚举类型为前缀。
11. 【强制】图片统一放在Assets.xcassets中，assets的命名统一为Assets.xcassets
12. 【强制】 方法注释必须描述方法的功能、参数说明、返回值说明
13. property属性修饰词【强制】如果是interface文件的属性，如果不想被外界修改，请必须使用readonly修饰
14. 【强制】property属性中必须使用特定的内存管理修饰词；内存管理默认修饰词：ARC下，基本类型是assign、OC对象是strong；NSString、NSArray、NSDictionary 一般情况下推荐使用copy修饰；NSMutableArray等可变容器property一般情况下使用strong修饰，不使用copy修饰；copy修饰后返回的是不可变数组；对象类型的property不推荐使用assign修饰；delegate推荐使用weak修饰，不应使用assign；block推荐使用copy修饰，虽然目前都是使用ARC，但是为了保持block的特性，我们都用copy修饰
15. 【强制】整形的转换为BOOL型的时候要小心。BOOL在Objective-C里被定义为signed char，这意味着它不仅仅只有YES(1)和NO(0)两个值。不要直接把整形强制转换为BOOL型。
16. 【强制】请勿直接将BOOL变量直接与YES进行比较。YES被定义为1，BOOL在Objective-C里被定义为signed char，所以可能会出现不相等的情况；
17. 【强制】属性、方法参数、方法返回值、block参数和返回值都必须指定可空性修饰词
18. 【强制】if-else书写规范：不管if或者else中的代码块有多少，都应该放在大括号 {} 中；else紧跟在if块的右 } 之后，不要另起一行；多条件时，尽量存在有else，对其他未定义条件进行容错处理
19. 【强制】如果存在if嵌套，禁止>=3层的if嵌套，如果存在请重构。采用if平铺方式进行处理复杂业务逻辑-卫语句原则。将失败前置原则：在处理if表达式时，将失败前置，将核心正常流程写在异常处理之后
20. 【强制】禁止使用宏来定义常量；如果要定义常量请参考上述的常量定义规范
21. 【强制】宏定义中如果包含表达式或变量，表达式和变量必须用小括号括起来;
22. 【强制】禁止滥用单例模式，单例使用需提前和leader沟通；因为单例一旦创建将会和app的生命周期一样，内存将不被释放，不受控制会导致内存暴增
23. 【强制】通知名的定义；禁止使用#deine来定义通知名；禁止在postNotificationName直接使用字符串来发送通知；通知命名规范：k+ 前缀 + [触发通知的类名] + [Did | Will] | [动作] + Notification；
24. 【强制】通知在哪个线程发送，就会在哪个线程接收，注意接收方法中的UI操作必须回到主线程中；
25. 【强制】NSNotification的object和userInfo为nullable，请在接收通知后，如果使用两者接收数据，请判断是否为nil
26. 【强制】switch的书写规范；每一个case下始终都要有大括号{},大括号{}中的{和case同一行，紧跟着:后面，之间留一个空格。在一个switch块内，都必须包含一个default语句并且放在最后，即使它什么代码也没有。
27. 【强制】我们在遍历可变数组时，不要做增删数组中元素的操作。因为删除操作可能会引起数组容量的变化，导致数组越界等问题
28. 【强制】不能在遍历时，对字典进行增删操作，请创建一个字典来保存待删除元素，待遍历结束后，对原集合进行增删处理
29. 【强制】block中注意循环引用的问题；
    * mweakify(self)和mstrongify(self);来处理block循环引用的问题;
    * block嵌套时，mweakify(self)和mstrongify(self)必须成对出现，不允许出现一个mweakify(self)对应多个mstrongify(self);
30. 【强制】block调用注意对block为空判断，推荐使用宏m_safe_call_block_mainThread 和 m_safe_call_block调用无返回值的block，如果有返回值，必须判断block是否为空，再调用block
31. 【强制】不能使用NSUserDefault
32. 【强制】iOS的UI操作必须要在主线程进行
33. 【强制】业务代码需要多线程同步，但又不具备高并发的特点，因此对锁的性能不是很敏感，使用@synchronized同步块。
34. 【强制】如果使用dispatch_semaphore_t实现锁的功能，dispatch_semaphore_wait的超时参数不可以设置为DISPATCH_TIME_FOREVER，
可以设置成一个固定的超时时间，使用者需要处理超时情况的发生。



