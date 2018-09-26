
生命周期 Lifecycle

1. 前言
    地址
    Demo

1. 如何简单的使用？

    简要介绍


2. 如何回调生命周期的？

    Lifecycle 代表生命周期事件与状态
    LifecycleOwner 代表生命周期的所有者     <- SupportActivity 
        getLifecyle => return Lifecycle     ReportFragment
    
    回调生命周期的类
        FragmentActivity
        SupportActivity
        ReportFragment

    为什么会在FragmentActivity和SupportActivity中处理生命周期状态呢？
        处理停止事件
            当Lifecycle属于AppCompatActivity或Fragment时，
            当AppCompatActivity或Fragment的onSaveInstanceState()被调用时，
            会将Lifecycle的状态改变至CREATE，同时ON_STOP事件会被分发。

            当Framgent或AppCompatActivity的状态通过onSaveInstanceState()被存储时，
            在ON_START被调用前，UI被认为是不可以被改变的。
            保存状态后尝试修改UI可能会导致应用程序的导航状态不一致，这就是为什么FragmentManager会在保存状态后运行FragmentTransaction时抛出异常的原因。

            如果观察者的关联生命周期还没有到至少STARTED状态，LiveData会通过禁止调用其观察者来防止这种边缘情况开箱即用。在幕后，它在决定调用其观察者之前调用isAtLeast（）。

            不幸的是，在onSaveInstanceState（）之后调用AppCompatActivity的onStop（）方法，这留下了一个间隙，其中不允许UI状态更改，但生命周期尚未移至CREATED状态。

    ReportFragment
        在Activity中加入空Fragment，用于监听生命周期状态变化

    LifecycleRegistry
        生周期状态管理与注册


3. 延迟注册Lifecycle时，如何调用生命周期的

4. Lifecycle的初始化流程

    SupportActiivty -> onCreate()

    ProcessLifecycleOwnerInitializer
        onCreate
            LifecycleDispatcher -> init
                                    DispatcherActivityCallback -> onActivityCreated()   