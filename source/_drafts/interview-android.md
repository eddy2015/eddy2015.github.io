---
title: Android 面试：Android 简述题
date: 2016-10-26 11:06:15
categories:
- Android 面试
tags:
- Android 面试
---

本文收集整理了 Android 面试中会遇到与 Android 知识相关的简述题。<!--more-->

## Android 的四大组件

## 四大组件的具体作用以及用法





## 介绍Activity、Service、Broadcast、BroadcastReceiver、Intent、IntentFilter

Activity :

应用程序中，一个Activity通常就是一个单独的屏幕，它上面可以显示一些控件也可以监听并处理用户的事件做出响应。

BroadcastReceiver广播接收器:

应用程序可以使用它对外部事件进行过滤只对感兴趣的外部事件(如当电话呼入时，或者数据网络可用时)进行接收并做出响应。广播接收器没有用户界面。然而，它们可以启动一个activity或serice 来响应它们收到的信息，或者用NotificationManager 来通知用户。通知可以用很多种方式来吸引用户的注意力──闪动背灯、震动、播放声音等。一般来说是在状态栏上放一个持久的图标，用户可以打开它并获取消息。

Service 服务:

一个Service 是一段长生命周期的，没有用户界面的程序，可以用来开发如监控类程序。

Content Provider内容提供者 :

android平台提供了Content Provider使一个应用程序的指定数据集提供给其他应用程序。这些数据可以存储在文件系统中、在一个SQLite数据库、或以任何其他合理的方式。其他应用可以通过ContentResolver类(见ContentProviderAccessApp例子)从该内容提供者中获取或存入数据.(相当于在应用外包了一层壳),只有需要在多个应用程序间共享数据是才需要内容提供者。例如，通讯录数据被多个应用程序使用，且必须存储在一个内容提供者中。

它的好处:统一数据访问方式。

# Activity

## Activity 生命周期

- 启动Activity：onCreate->onStart->onResume
- 锁屏或被其它Activity覆盖：onPause->onStop
- 解锁或由被覆盖状态再回到前台：onRestart->onStart->onResume
- 跳转到其它Activity或按Home进入后台：onPause->onStop
- 退回到此Activity：onRestart->onStart->onResume
- 退出此Activity：onPause->onStop->onDestory
- 对话框弹出不会执行任何生命周期(注：对话框如果是Activity(Theme为Dialog)，还是会执行生命周期的)
- ​
- 从A跳转到B：当B的主题为透明时，A只会执行onPause（A-onPause->B-(onCreate->onStart->onResume)）
- 从A跳转到B：A-onPause->B-(onCreate->onStart->onResume)-A-onStop(注意是A执行onPause后开始执行B的生命周期，B执行onResume后，A才执行onStop，所以尽量不要在onPause中做耗时操作)
- 从B返回到A：B-onPause->A-(onRestart->onStart->onResume)-B-(onStop->onDestroy)

![](http://o9sn2y8lr.bkt.clouddn.com/16-10-25/30416013.jpg)

## Activity和Fragment生命周期有哪些？

```java
Activity——onCreate->onStart->onResume->onPause->onStop->onDestroy

Fragment——onAttach->onCreate->onCreateView->onActivityCreated->onStart->onResume->onPause->onStop->onDestroyView->onDestroy->onDetach
```

## Activity四种启动模式的区别

## LanchMode 的应用场景

- standard 模式

这是默认模式，每次激活Activity时都会创建Activity实例，并放入任务栈中。

- singleTop 模式

如果在任务的栈顶正好存在该Activity的实例，就重用该实例( 会调用实例的 onNewIntent() )，否则就会创建新的实例并放入栈顶，即使栈中已经存在该Activity的实例，只要不在栈顶，都会创建新的实例。

- singleTask 模式

如果在栈中已经有该Activity的实例，就重用该实例(会调用实例的 onNewIntent() )。重用时，会让该实例回到栈顶，因此在它上面的实例将会被移出栈。如果栈中不存在该实例，将会创建新的实例放入栈中。

- singleInstance 模式

在一个新栈中创建该Activity的实例，并让多个应用共享该栈中的该Activity实例。一旦该模式的Activity实例已经存在于某个栈中，任何应用再激活该Activity时都会重用该栈中的实例( 会调用实例的 onNewIntent() )。其效果相当于多个应用共享一个应用，不管谁激活该 Activity 都会进入同一个应用中。

设置启动模式的位置在 AndroidManifest.xml 文件中 Activity 元素的 Android:launchMode 属性。

singleTop适合接收通知启动的内容显示页面。

例如，某个新闻客户端的新闻内容页面，如果收到10个新闻推送，每次都打开一个新闻内容页面是很烦人的。

singleTask适合作为程序入口点。

例如浏览器的主界面。不管从多少个应用启动浏览器，只会启动主界面一次，其余情况都会走onNewIntent，并且会清空主界面上面的其他页面。

singleInstance应用场景：

闹铃的响铃界面。 你以前设置了一个闹铃：上午6点。在上午5点58分，你启动了闹铃设置界面，并按 Home 键回桌面；在上午5点59分时，你在微信和朋友聊天；在6点时，闹铃响了，并且弹出了一个对话框形式的 Activity(名为 AlarmAlertActivity) 提示你到6点了(这个 Activity 就是以 SingleInstance 加载模式打开的)，你按返回键，回到的是微信的聊天界面，这是因为 AlarmAlertActivity 所在的 Task 的栈只有他一个元素， 因此退出之后这个 Task 的栈空了。如果是以 SingleTask 打开 AlarmAlertActivity，那么当闹铃响了的时候，按返回键应该进入闹铃设置界面。

## Activity中类似onCreate、onStart运用了哪种设计模式，优点是什么

模板模式。每次新建一个Actiivty时都会覆盖onCreate，onStart等方法,这些方法在父类中就相当于一个模板

## 如何将一个Activity设置成窗口的样式

- 在AndroidManifest.xml文件中设置当前需要改变成窗口样式的Activity的属性，即

```
android:theme="@android:style/Theme.Dialog"  
```

- 在styles.xml文件中自定义一个主题样式，改主题样式必须继承Dialog的样式.

## Activity的启动过程

1. 无论是通过Launcher来启动Activity，还是通过Activity内部调用startActivity接口来启动新的Activity，都通过Binder进程间通信进入到ActivityManagerService进程中，并且调用ActivityManagerService.startActivity接口； 
2. ActivityManagerService调用ActivityStack.startActivityMayWait来做准备要启动的Activity的相关信息；
3. ActivityStack通知ApplicationThread要进行Activity启动调度了，这里的ApplicationThread代表的是调用ActivityManagerService.startActivity接口的进程，对于通过点击应用程序图标的情景来说，这个进程就是Launcher了，而对于通过在Activity内部调用startActivity的情景来说，这个进程就是这个Activity所在的进程了；
4. ApplicationThread不执行真正的启动操作，它通过调用ActivityManagerService.activityPaused接口进入到ActivityManagerService进程中，看看是否需要创建新的进程来启动Activity；
5. 对于通过点击应用程序图标来启动Activity的情景来说，ActivityManagerService在这一步中，会调用startProcessLocked来创建一个新的进程，而对于通过在Activity内部调用startActivity来启动新的Activity来说，这一步是不需要执行的，因为新的Activity就在原来的Activity所在的进程中进行启动；
6. ActivityManagerServic调用ApplicationThread.scheduleLaunchActivity接口，通知相应的进程执行启动Activity的操作；
7. ApplicationThread把这个启动Activity的操作转发给ActivityThread，ActivityThread通过ClassLoader导入相应的Activity类，然后把它启动起来。



## windows和activity之间关系？

## WindowManager 的相关知识

## Activity、Window 和 View 三者的区别

- 一个 Activity 构造的时候一定会构造一个 Window(PhoneWindow)，并且只有一个
- 这个Window会有一个ViewRoot(View、ViewGroup)
- 通过addView()加载布局
- WindowMangerService 接收消息，并且回到 Activity 函数，比如onKeyDown()

Activity 是控制单元，Window 是承载模型，View 是显示视图

## 一个activity打开另外一个activity，再打开一个activity？回去的时候发生了什么操作？



## onActivityResult(int requestCode, int resultCode, Intent data)方法的用法；

如果你想在Activity中得到新打开Activity关闭后返回的数据，你需要使用系统提供的startActivityForResult(Intent intent,int requestCode)方法打开新的Activity，新的Activity关闭后会向前面的Activity传回数据，为了得到传回的数据，你必须在前面的Activity中重写onActivityResult(int requestCode, int resultCode,Intent data)方法。

## 不用Service,B页面为音乐播放，从A跳到B，再返回，如何使音乐继续播放？

A使用startActivityForResult方法开启B，B类结束时调用finish;A类的Intent有一个子Activity结束事件onActivityResult，在这个事件里继续播放音乐。

## 内存不足时，怎么保持Activity的一些状态，在哪个方法里面做具体操作？

在onSaveInstanceState方法中保存Activity的状态，在onRestoreInstanceState或onCreate方法中恢复Activity的状态

## onSaveInstanceState方法

- 用于保存Activity的状态存储一些临时数据
- Activity被覆盖或进入后台，由于系统资源不足被kill会被调用
- 用户改变屏幕方向会被调用
- 跳转到其它Activity或按Home进入后台会被调用
- 会在onStop之前被调用，和onPause的顺序不固定的

## onRestoreInstanceState(Bundle savedInstanceState)方法

- 用于恢复保存的临时数据，此方法的Bundle参数也会传递到onCreate方法中，你也可以在onCreate(Bundle savedInstanceState)方法中恢复数据
- onRestoreInstanceState和onCreate的区别：当onRestoreInstanceState被调用时Bundle参数一定是有值的，不用做为null判断，onCreate的Bundle则可能会为null。官方文档建议在此方法中进行数据恢复。
- 由于系统资源不足被kill之后又回到此Activity会被调用
- 用户改变屏幕方向重建Activity时会被调用
- 会在onStart之后被调用

## 同一个程序不同的Activity如何放在不同的任务栈中？

需要为不同的activity设置不同的affinity属性，启动activity的Intent需要包含FLAG_ACTIVITY_NEW_TASK标记。

## 如何安全退出已调用多个Activity的Application？

-记录打开的Activity：每打开一个Activity，就记录下来。在需要退出时，关闭每一个Activity即可。
-发送特定广播：在需要结束应用时，发送一个特定的广播，每个Activity收到广播后，关闭即可。
-递归退出：在打开新的Activity时使用startActivityForResult，然后自己加标志，在onActivityResult中处理，递归关闭。

# Fragment

## Fragment生命周期

![](http://o9sn2y8lr.bkt.clouddn.com/16-10-19/72771704.jpg)



![](http://o9sn2y8lr.bkt.clouddn.com/16-10-27/49568023.jpg)

## Activity中如何动态的添加Fragment？

```java
//向活动添加碎片,根据屏幕的纵向和横向显示
//1,获取碎片管理器
FragmentManager fragment=getFragmentManager();
//2,碎片的显示需要使用FragmentTransaction类操作
FragmentTransaction transacction=fragment.beginTransaction();
//获取屏幕管理器和默认的显示
Display display=getWindowManager().getDefaultDisplay();
//判断横屏
if(display.getWidth()>display.getHeight()){
	//获取java类
	Frament1 frament1 =  new Frament1();
	transacction.replace(android.R.id.content, frament1);
}else{
	Frament2 frament2 =  new Frament2();
	transacction.replace(android.R.id.content, frament2);
}
//使用FragmentTransaction必须要commit
transacction.commit();
```

## Fragment 特点

-Fragment可以作为Activity界面的一部分组成出现；
-可以在一个Activity中同时出现多个Fragment，并且一个Fragment也可以在多个Activity中使用；
-在Activity运行过程中，可以添加、移除或者替换Fragment；
-Fragment可以响应自己的输入事件，并且有自己的生命周期，它们的生命周期会受宿主Activity的生命周期影响。
-Fragment可以轻松得创建动态灵活的UI设计，可以适应于不同的屏幕尺寸。从手机到平板电脑。
-Fragment 解决Activity间的切换不流畅，轻量切换。
-Fragment 替代TabActivity做导航，性能更好。
-Fragment做局部内容更新更方便，原来为了到达这一点要把多个布局放到一个activity里面，现在可以用多Fragment来代替，只有在需要的时候才加载Fragment，提高性能。

## Fragment嵌套多个Fragment会出现bug吗？

参考：http://blog.csdn.net/megatronkings/article/details/51417510

# Service

## 什么是Service以及描述下它的生命周期。

Service是运行在后台的android组件，没有用户界面，不能与用户交互，可以运行在自己的进程，也可以运行在其他应用程序的上下
文里。

Service随着启动形式的不同，其生命周期稍有差别。当用Context.startService()来启动时，Service的生命周期依次为:oncreate——>onStartCommand——>onDestroy 

当用Context.bindService()启动时:onStart——>onBind——>onUnbind——>onDestroy

![](http://o9sn2y8lr.bkt.clouddn.com/16-10-25/20789048.jpg)

## Service有哪些启动方法，有什么区别，怎样停用Service？

Service启动方式有两种；一是Context.startService和Context.bindService。
   区别是通过startService启动时Service组件和应用程序没多大的联系；当用访问者启动之后，如果访问者不主动关闭，Service就不会关闭，Service组件之间
因为没什么关联，所以Service也不能和应用程序进行数据交互。而通过bindService进行绑定时，应用程序可以通过ServiceConnection进行数据交互。在实现Service
时重写的onBind方法中，其返回的对象会传给ServiceConnection对象的onServiceConnected(ComponentName name, IBinder service)中的service参数；也就是说获取
了serivce这个参数就得到了Serivce组件返回的值。Context.bindService(Intent intent,ServiceConnection conn,int flag)其中只要与Service连接成功
conn就会调用其onServiceConnected方法

   停用Service使用Context.stopService

## 注册Service需要注意什么

无论使用哪种启动方法，都需要在xml里注册你的Service，就像这样:

```
<service
        android:name=".packnameName.youServiceName"
        android:enabled="true" />
```

## service 可以执行耗时操作吗

不能

## Service和Activity在同一个线程吗

默认情况下是在同一个主线程中。但可以通过配置android:process=":remote" 属性让 Service 运行在不同的进程。

## Service与Activity怎么实现通信

-通过Binder对象

当Activity通过调用bindService(Intent service, ServiceConnection conn,int flags),我们可以得到一个Service的一个对象实例，然后我们就可以访问Service中的方法

-Activity调用bindService (Intent service, ServiceConnection conn, int flags)方法，得到Service对象的一个引用，这样Activity可以直接调用到Service中的方法，如果要主动通知Activity，我们可以利用回调方法
-Service向Activity发送消息，可以使用广播，当然Activity要注册相应的接收器。比如Service要向多个Activity发送同样的消息的话，用这种方法就更好。

## Service里面可以弹出 dialog 或 Toast 吗

可以。`getApplicationContext()`

## Service 上能不能弹出对话框

## 什么时候使用Service?

比如播放多媒体的时候用户启动了其他的Activity这个时候程序要在后台继续播放，比如检测SD卡上文件的变化，在或者在后台记录你地理位置的改变等等

## 说说Activity、Intent、Service是什么关系

一个Activity通常是一个单独的屏幕，每一个Activity都被实现为一个单独的类，这些类都是从Activity基类中继承来的，Activity类显示有视图控件组成的用户接口，并对视图控件的事件做出响应。

Intent的调用是用来进行架构屏幕之间的切换的。Intent是描述应用想要做什么。Intent数据结果中最重要的部分是动作和动作对应的数据，一个动作对应一个动作数据。

Service是运行在后台的代码，不能与用户交互，可以运行在自己的进程，也可以运行在其他应用程序的上下文里。需要通过某一个Activity或其他Context对象来调用。 

Activity 跳转到Activity,Activtiy启动Service,Service打开Activity都需要Intent表明跳转的意图，以及传递参数，Intent是这些组件间信号传递的传承者。

## 怎么在启动一个activity时就启动一个service

在activity的onCreate里写
startService(xxx);
然后
this.finish();结束自己..
这是最简单的方法 可能会有屏幕一闪的现象，如果UI要求严格的话用AIDL把

## Service 和 Activity 之间通讯的几种方式

-通过 Binder 对象。
-通过 broadcast(广播) 的形式。

Binder 相关知识参考：http://blog.csdn.net/boyupeng/article/details/47011383

## 为什么在Service中创建子线程而不是Activity中

这是因为Activity很难对Thread进行控制，当Activity被销毁之后，就没有任何其它的办法可以再重新获取到之前创建的子线程的实例。而且在一个Activity中创建的子线程，另一个Activity无法对其进行操作。但是Service就不同了，所有的Activity都可以与Service进行关联，然后可以很方便地操作其中的方法，即使Activity被销毁了，之后只要重新与Service建立关联，就又能够获取到原有的Service中Binder的实例。因此，使用Service来处理后台任务，Activity就可以放心地finish，完全不需要担心无法对后台任务进行控制的情况。

## 如何保证 Service 在后台不被 kill

-onStartCommand方法，返回START_STICKY

START_STICKY 在运行onStartCommand后service进程被kill后，那将保留在开始状态，但是不保留那些传入的intent。不久后service就会再次尝试重新创建，因为保留在开始状态，在创建     service后将保证调用onstartCommand。如果没有传递任何开始命令给service，那将获取到null的intent。

START_NOT_STICKY 在运行onStartCommand后service进程被kill后，并且没有新的intent传递给它。Service将移出开始状态，并且直到新的明显的方法（startService）调用才重新创建。因为如果没有传递任何未决定的intent那么service是不会启动，也就是期间onstartCommand不会接收到任何null的intent。

START_REDELIVER_INTENT 在运行onStartCommand后service进程被kill后，系统将会再次启动service，并传入最后一个intent给onstartCommand。直到调用stopSelf(int)才停止传递intent。如果在被kill后还有未处理好的intent，那被kill后服务还是会自动启动。因此onstartCommand不会接收到任何null的intent。

-提升service优先级

在AndroidManifest.xml文件中对于intent-filter可以通过android:priority = "1000"这个属性设置最高优先级，1000是最高值，如果数字越小则优先级越低，同时适用于广播。

-提升service进程优先级

Android中的进程是托管的，当系统进程空间紧张的时候，会依照优先级自动进行进程的回收。Android将进程分为6个等级,它们按优先级顺序由高到低依次是:

1.前台进程( FOREGROUND_APP)
2.可视进程(VISIBLE_APP )
3.次要服务进程(SECONDARY_SERVER )
4.后台进程 (HIDDEN_APP)
5.内容供应节点(CONTENT_PROVIDER)
6.空进程(EMPTY_APP)

当service运行在低内存的环境时，将会kill掉一些存在的进程。因此进程的优先级将会很重要，可以使用startForeground 将service放到前台状态。这样在低内存时被kill的几率会低一些。

-onDestroy方法里重启service

service +broadcast  方式，就是当service走ondestory的时候，发送一个自定义的广播，当收到广播的时候，重新启动service；

-Application加上Persistent属性
-监听系统广播判断Service状态

通过系统的一些广播，比如：手机重启、界面唤醒、应用状态改变等等监听并捕获到，然后判断我们的Service是否还存活，别忘记加权限啊。

## 什么是IntentService？有何优点？

IntentService也是一个Service，是Service的子类；
IntentService和Service有所不同，通过Looper和Thread来解决标准Service中处理逻辑的阻塞的问题
优点：Activity的进程，当处理Intent的时候，会产生一个对应的Service,Android的进程处理器现在会尽可能的不kill掉你。

**IntentService的使用场景与特点。**

> IntentService是Service的子类，是一个异步的，会自动停止的服务，很好解决了传统的Service中处理完耗时操作忘记停止并销毁Service的问题

优点：

- 一方面不需要自己去new Thread
- 另一方面不需要考虑在什么时候关闭该Service

onStartCommand中回调了onStart，onStart中通过mServiceHandler发送消息到该handler的handleMessage中去。最后handleMessage中回调onHandleIntent(intent)。

# ContentProvider

参考：[http://blog.csdn.net/coder_pig/article/details/47858489](http://blog.csdn.net/coder_pig/article/details/47858489)

## ContentProvider 简介

ContentProvider(内容提供者)：为存储和获取数据提供统一的接口。可以在不同的应用程序之间共享数据。Android已经为常见的一些数据提供了默认的ContentProvider

1.ContentProvider 为存储和读取数据提供了统一的接口
2.使用ContentProvider，应用程序可以实现 app 间数据共享
3.Android内置的许多数据都是使用ContentProvider形式，供开发者调用的(如视频，音频，图片，通讯录等)

## 请介绍下ContentProvider是如何实现数据共享的

创建一个属于你自己的Content provider或者将你的数据添加到一个已经存在的Content provider中，前提是有相同数据类型并且有写入Content provider的权限。

## ContentProvider使用方法

参考：[http://blog.csdn.net/juetion/article/details/17481039](http://blog.csdn.net/juetion/article/details/17481039)

# Broadcast

## 注册广播有哪几种方式,有什么区别

-在应用程序的代码中注册

```java
registerReceiver（receiver，filter）；// 注册BroadcastReceiver

unregisterReceiver（receiver）；// 取消注册BroadcastReceiver
```

当BroadcastReceiver更新UI，通常会使用这样的方法注册。启动Activity时候注册BroadcastReceiver，Activity不可见时候，取消注册。

-在androidmanifest.xml当中注册

```java
<receiver>

    <intent-filter>

     <action Android:name = "android.intent.action.PICK"/>

    </intent-filter>

</receiver>
```

1)第一种不是常驻型广播，也就是说广播跟随程序的生命周期。

2)第二种是常驻型，也就是说当应用程序关闭后，如果有信息广播来，程序也会被系统调用自动运行。

- 静态注册：在AndroidManifest.xml文件中进行注册，当App退出后，Receiver仍然可以接收到广播并且进行相应的处理
- 动态注册：在代码中动态注册，当App退出后，也就没办法再接受广播了

## Android引入广播机制的用意？

## 无序广播、有序广播

## 本地广播和全局广播有什么差别

全局广播 BroadcastReceiver 是针对应用间、应用与系统间、应用内部进行通信的一种方式。

本地广播 LocalBroadcastReceiver 仅在自己的应用内发送接收广播，也就是只有自己的应用能收到。因广播数据在本应用范围内传播，不用担心隐私数据泄露的问题。 不用担心别的应用伪造广播，造成安全隐患。 相比在系统内发送全局广播，它更高效。

## 关于一个 app 被杀掉进程后，是否还能收到广播



# Intent

**Intent的使用方法，可以传递哪些数据类型。**

通过查询Intent/Bundle的API文档，我们可以获知，Intent/Bundle支持传递基本类型的数据和基本类型的数组数据，以及String/CharSequence类型的数据和String/CharSequence类型的数组数据。而对于其它类型的数据貌似无能为力，其实不然，我们可以在Intent/Bundle的API中看到Intent/Bundle还可以传递Parcelable（包裹化，邮包）和Serializable（序列化）类型的数据，以及它们的数组/列表数据。

所以要让非基本类型和非String/CharSequence类型的数据通过Intent/Bundle来进行传输，我们就需要在数据类型中实现Parcelable接口或是Serializable接口。

[http://blog.csdn.net/kkk0526/article/details/7214247](http://blog.csdn.net/kkk0526/article/details/7214247)

# Context

## Context区别

- Activity和Service以及Application的Context是不一样的,Activity继承自ContextThemeWraper.其他的继承自ContextWrapper
- 每一个Activity和Service以及Application的Context都是一个新的ContextImpl对象
- getApplication()用来获取Application实例的，但是这个方法只有在Activity和Service中才能调用的到。那么也许在绝大多数情况下我们都是在Activity或者Service中使用Application的，但是如果在一些其它的场景，比如BroadcastReceiver中也想获得Application的实例，这时就可以借助getApplicationContext()方法，getApplicationContext()比getApplication()方法的作用域会更广一些，任何一个Context的实例，只要调用getApplicationContext()方法都可以拿到我们的Application对象。
- Activity在创建的时候会new一个ContextImpl对象并在attach方法中关联它，Application和Service也差不多。ContextWrapper的方法内部都是转调ContextImpl的方法
- 创建对话框传入Application的Context是不可以的
- 尽管Application、Activity、Service都有自己的ContextImpl，并且每个ContextImpl都有自己的mResources成员，但是由于它们的mResources成员都来自于唯一的ResourcesManager实例，所以它们看似不同的mResources其实都指向的是同一块内存
- Context的数量等于Activity的个数 + Service的个数 + 1，这个1为Application

#  Handler

## Handler 原理

andriod提供了Handler 和 Looper 来满足线程间的通信。Handler先进先出原则。Looper类用来管理特定线程内对象之间的消息交换(MessageExchange)。

Looper: 一个线程可以产生一个Looper对象，由它来管理此线程里的MessageQueue(消息队列)。

Handler: 你可以构造Handler对象来与Looper沟通，以便push新消息到MessageQueue里;或者接收Looper从Message Queue取出)所送来的消息。

Message Queue(消息队列):用来存放线程放入的消息。

线程：UIthread 通常就是main thread，而Android启动程序时会替它建立一个MessageQueue。

参考：[Handler、Looper、Message、MessageQueue基础流程分析](https://github.com/GeniusVJR/LearningNotes/blob/master/Part1/Android/%E7%BA%BF%E7%A8%8B%E9%80%9A%E4%BF%A1%E5%9F%BA%E7%A1%80%E6%B5%81%E7%A8%8B%E5%88%86%E6%9E%90.md)

## Handler消息机制，postDelayed会造成线程阻塞吗？对内存有什么影响？



## Handler、Thread 和 HandlerThread 的区别

参考：http://www.juwends.com/tech/android/android_thread_handler.html

Handler 会关联一个单独的线程和消息队列。Handler默认关联主线程(即 UI 线程)，虽然要提供Runnable参数 ，但默认是直接调用Runnable中的run()方法。也就是默认下会在主线程执行，如果在这里面的操作会有阻塞，界面也会卡住。如果要在其他线程执行，可以使用 HandlerThread。

HandlerThread继承于Thread，所以它本质就是个Thread。与普通Thread的差别就在于，主要的作用是建立了一个线程，并且创立了消息队列，有自己的looper,可以让我们在自己的线程中分发和处理消息。

android.os.Handler可以通过Looper对象实例化，并运行于另外的线程中，Android提供了让Handler运行于其它线程的线程实现，也是就HandlerThread。HandlerThread对象start后可以获得其Looper对象，并且使用这个Looper对象实例Handler。

## 使用 Handler 时怎么避免引起内存泄漏

参考：[Handler内存泄漏分析及解决](http://www.jianshu.com/p/cb9b4b71a820)

一旦Handler被声明为内部类，那么可能导致它的外部类不能够被垃圾回收。如果Handler是在其他线程（我们通常成为worker 
thread）使用Looper或MessageQueue（消息队列），而不是main线程（UI线程），那么就没有这个问题。如果Handler使用
Looper或MessageQueue在主线程（main thread），你需要对Handler的声明做如下修改： 

声明Handler为static类；在外部类中实例化一个外部类的WeakReference（弱引用）并且在Handler初始化时传入这个对象给你的Handler；将所有引用的外部类成员使用WeakReference对象。

```java
private static class MyHandler extends Handler {          private final WeakReference<HandlerActivity2> mActivity;            public MyHandler(HandlerActivity2 activity) {              mActivity = new WeakReference<HandlerActivity2>(activity);          }            @Override          public void handleMessage(Message msg) {              System.out.println(msg);              if (mActivity.get() == null) {                  return;              }              mActivity.get().todo();          }      }  
```

  当Activity finish后 handler对象还是在Message中排队。 还是会处理消息，这些处理有必要？
  正常Activitiy finish后，已经没有必要对消息处理，那需要怎么做呢？
  解决方案也很简单，在Activity onStop或者onDestroy的时候，取消掉该Handler对象的Message和Runnable。
  通过查看Handler的API，它有几个方法：removeCallbacks(Runnable r)和removeMessages(int what)等。

```java
/**    * 一切都是为了不要让mHandler拖泥带水    */   @Override   public void onDestroy() {       mHandler.removeMessages(MESSAGE_1);       mHandler.removeMessages(MESSAGE_2);       mHandler.removeMessages(MESSAGE_3);         // ... ...         mHandler.removeCallbacks(mRunnable);         // ... ...   }  
```

# AsyncTask


## AsyncTask原理



# 线程

## 子线程中更新UI的方法

第一种：Handler+Message

第二种：

```
new Handler(context.getMainLooper()).post(
  new Runnable(){
    public void run(){
    //更新UI
    }
  });
```

第三种：

```
((Activity)context)runOnUiThread(
  new Runnable(){
    public void run(){
      //更新UI
    }
  }
  );
```

## 多线程

- 继承 Thread 类
- 实现 Runnable 接口
- HandlerThread
- Handler
- AsyncTask
- Activity.runOnUiThread(Runnable)
- View.post(Runnable),View.postDelay(Runnable,long)

## 线程同步

参考：http://www.itzhai.com/java-based-notebook-thread-synchronization-problem-solving-synchronization-problems-synchronized-block-synchronized-methods.html#2.1%E3%80%81%E4%BD%BF%E7%94%A8synchronized%E5%85%B3%E9%94%AE%E5%AD%97%E5%88%9B%E5%BB%BAsynchronized%E6%96%B9%E6%B3%95%EF%BC%9A

http://www.juwends.com/tech/android/android-inter-thread-comm.html

- 使用 synchronized 关键字创建 synchronized 方法。
- 使用 synchronized 创建同步代码块。

# 进程

## 进程优先级

1. 前台进程：即与用户正在交互的Activity或者Activity用到的Service等，如果系统内存不足时前台进程是最后被杀死的
2. 可见进程：可以是处于暂停状态(onPause)的Activity或者绑定在其上的Service，即被用户可见，但由于失去了焦点而不能与用户交互
3. 服务进程：其中运行着使用startService方法启动的Service，虽然不被用户可见，但是却是用户关心的，例如用户正在非音乐界面听的音乐或者正在非下载页面自己下载的文件等；当系统要空间运行前两者进程时才会被终止
4. 后台进程：其中运行着执行onStop方法而停止的程序，但是却不是用户当前关心的，例如后台挂着的QQ，这样的进程系统一旦没了有内存就首先被杀死
5. 空进程：不包含任何应用程序的程序组件的进程，这样的进程系统是一般不会让他存在的

## IntentService 作用是什么，AIDL 解决了什么问题

IntentService 是继承于 Service 并处理异步请求的一个类，在IntentService 内有一个工作线程来处理耗时操作，启动 IntentService 的方式和启动传统 Service 一样，同时，当任务执行完后，IntentService 会自动停止，而不需要我们去手动控制。另外，可以启动 IntentService 多次，而每一个耗时操作会以工作队列的方式在 IntentService 的 onHandleIntent 回调方法中执行，并且，每次只会执行一个工作线程，执行完第一个再执行第二个，以此类推。

IntentService 与 Service的不同：

-直接 创建一个默认的工作线程,该线程执行所有的intent传递给onStartCommand()区别于应用程序的主线程。
-直接创建一个工作队列,将一个意图传递给你onHandleIntent()的实现,所以我们就永远不必担心多线程。
-当请求完成后自己会调用stopSelf()，所以你就不用调用该方法了。
-提供的默认实现onBind()返回null，所以也不需要重写这个方法。so easy啊
-提供了一个默认实现onStartCommand(),将意图工作队列,然后发送到你onHandleIntent()实现。真是太方便了

AIDL (Android Interface Definition Language) 是一种IDL 语言，用于生成可以在 Android 设备上两个进程之间进行进程间通信(interprocess communication, IPC)的代码。如果在一个进程中（例如Activity）要调用另一个进程中（例如 Service, 设置了属性 android:process=":remote" 后，Service 就会运行在另外一个进程）对象的操作，就可以使用AIDL生成可序列化的参数。 AIDL IPC机制是面向接口的，像COM或Corba一样，但是更加轻量级。它是使用代理类在客户端和实现端传递数据。



## AIDL的全称是什么？如何工作？能处理哪些类型的数据

ADIL是一种接口定义语言，用于约束两个进程之间的通信规则，供编译器生成代码，实现android设备之间的进程通信。

进程之间的通信信息首先会被转换成AIDL协议消息，然后发送给对方，对方受到AIDL协议消息后在转换成相应的对象。AIDL支持类型包括java基础类型和String，List，Map,CharSequence,如果使用自定类型，必须实现Parcelable接口

## Broadcast、Content Provider 和 AIDL的区别和联系

这3种都可以实现跨进程的通信，那么从效率，适用范围，安全性等方面来比较的话他们3者之间有什么区别？

Broadcast：用于发送和接收广播！实现信息的发送和接收！
AIDL：用于不同程序间服务的相互调用！实现了一个程序为另一个程序服务的功能！
Content Provider:用于将程序的数据库人为地暴露出来！实现一个程序可以对另个程序的数据库进行相对用的操作！

Broadcast,既然是广播，那么它的优点是：注册了这个广播接收器的应用都能够收到广播，范围广。
缺点是：速度慢点，而且必须在一定时间内把事情处理完(onReceive执行必须在几秒之内)，否则的话系统给出ANR。

AIDL，是进程间通信用的，类似一种协议吧。优点是：速度快(系统底层直接是共享内存)，性能稳，效率高，一般进程间通信就用它。

Content Provider,因为只是把自己的数据库暴露出去，其他程序都可以来获取数据，数据本身不是实时的，不像前两者,只是起个数据供应作用。一般是某个成熟的应用来暴露自己的数据用的。 你要是为了进程间通信，还是别用这个了，这个又不是实时数据。

## 进程间传输方式

## Android进程间通信，Binder机制



# 触摸事件

## View 事件分发机制

WindowManager->window->Decorview->子 view。最后我说当所有的 view 都不处理事件，事件会最后会传递到 Activity 的 onTouchEvent 上

参考：[事件分发机制](http://www.jianshu.com/p/e99b5e8bd67b)

## Touch 事件传递流程

Android事件的基础知识：

所有的Touch事件都封装到MotionEvent里面

事件处理包括三种情况，分别为：传递—-dispatchTouchEvent()函数、拦截——onInterceptTouchEvent()函数、消费—-onTouchEvent()函数和OnTouchListener

事件类型分为ACTION_DOWN, ACTION_UP, ACTION_MOVE, ACTION_POINTER_DOWN, ACTION_POINTER_UP, ACTION_CANCEL等，每个事件都是以ACTION_DOWN开始ACTION_UP结束

Android事件传递流程：

事件都是从Activity.dispatchTouchEvent()开始传递

事件由父View传递给子View，ViewGroup可以通过onInterceptTouchEvent()方法对事件拦截，停止其向子view传递

如果事件从上往下传递过程中一直没有被停止，且最底层子View没有消费事件，事件会反向往上传递，这时父View(ViewGroup)可以进行消费，如果还是没有被消费的话，最后会到Activity的onTouchEvent()函数。

如果View没有对ACTION_DOWN进行消费，之后的其他事件不会传递过来，也就是说ACTION_DOWN必须返回true，之后的事件才会传递进来

OnTouchListener优先于onTouchEvent()对事件进行消费

![](http://o9sn2y8lr.bkt.clouddn.com/16-10-14/18162238.jpg)

![](http://o9sn2y8lr.bkt.clouddn.com/16-10-14/44984087.jpg)

![](http://o9sn2y8lr.bkt.clouddn.com/16-10-14/48753378.jpg)

## onInterceptTouchEvent()和onTouchEvent()的区别？

onInterceptTouchEvent()用于拦截触摸事件
onTouchEvent()用于处理触摸事件

# View

## View 绘制流程

参考：http://www.codekk.com/blogs/detail/54cfab086c4761e5001b253f

当 Activity 接收到焦点的时候，它会被请求绘制布局,该请求由 Android framework 处理.绘制是从根节点开始，对布局树进行 measure 和 draw。整个 View 树的绘图流程在ViewRoot.java类的performTraversals()函数展开，该函数所做 的工作可简单概况为是否需要重新计算视图大小(measure)、是否需要重新安置视图的位置(layout)、以及是否需要重绘(draw)，流程图如下：

![](http://o9sn2y8lr.bkt.clouddn.com/view_mechanism_flow.png)

![](http://o9sn2y8lr.bkt.clouddn.com/view_draw_method_chain.png)

## requertlayout onlayout onDraw drawChild 的区别和联系

- requestLayout()方法 ：会导致调用measure()过程 和 layout()过程 。 将会根据标志位判断是否需要ondraw
- onLayout()方法(如果该View是ViewGroup对象，需要实现该方法，对每个子视图进行布局)
- 调用onDraw()方法绘制视图本身   (每个View都需要重载该方法，ViewGroup不需要实现该方法)
- drawChild()去重新回调每个子视图的draw()方法

## invalidata() 和 postInvalidata() 的区别及使用

- invalidata() 必须在 UI 线程中调用，所以一般都是配合 Handler 使用。
- postInvalidata() 可以在其他线程直接调用。



## View 刷新机制

由 ViewRoot对象的performTraversals()方法调用draw()方法发起绘制该View树，值得注意的是每次发起绘图时，并不会重新绘制每个View树的视图，而只会重新绘制那些“需要重绘”的视图，View类内部变量包含了一个标志位DRAWN，当该视图需要重绘时，就会为该View添加该标志位。

调用流程 ：

mView.draw()开始绘制，draw()方法实现的功能如下：

1. 绘制该View的背景
2. 为显示渐变框做一些准备操作(见5，大多数情况下，不需要改渐变框)          
3. 调用onDraw()方法绘制视图本身   (每个View都需要重载该方法，ViewGroup不需要实现该方法)
4. 调用dispatchDraw ()方法绘制子视图(如果该View类型不为ViewGroup，即不包含子视图，不需要重载该方法)值得说明的是，ViewGroup类已经为我们重写了dispatchDraw ()的功能实现，应用程序一般不需要重写该方法，但可以重载父类函数实现具体的功能。

参考：http://blog.csdn.net/chenzhiqin20/article/details/8628952

View的绘制流程是从ViewRoot的performTraversals（）方法开始，依次经过measure（），layout（）和draw（）三个过程才最终将一个View绘制出来。

## Android 绘图机制原理



## postInvalidate与invalidate有什么区别？

- 都用于刷新界面
- postInvalidate()用在子线程
- invalidate()用在主线程

## notifyDataSetChanged和notifyDataSetInvalidated的区别

- notifyDataSetInvalidated()，会重绘整个控件（还原到初始状态）
- notifyDataSetChanged()，重绘当前可见区域

## SurfaceView和View的区别是什么？

SurfaceView中采用了双缓存技术，在单独的线程中更新界面。而View在UI线程中更新界面。

## RemoteView在哪些功能中使用

APPwidget和Notification中

# 自定义 View

## 自定义View相关方法

## 如何自定义控件

1. 自定义属性的声明和获取
   - 分析需要的自定义属性
   - 在res/values/attrs.xml定义声明
   - 在layout文件中进行使用
   - 在View的构造方法中进行获取
2. 测量onMeasure
3. 布局onLayout(ViewGroup)
4. 绘制onDraw
5. onTouchEvent
6. onInterceptTouchEvent(ViewGroup)
7. 状态的恢复与保存

## 优化自定义 View

为了加速你的view，对于频繁调用的方法，需要尽量减少不必要的代码。先从onDraw开始，需要特别注意不应该在这里做内存分配的事情，因为它会导致GC，从而导致卡顿。在初始化或者动画间隙期间做分配内存的动作。不要在动画正在执行的时候做内存分配的事情。

你还需要尽可能的减少onDraw被调用的次数，大多数时候导致onDraw都是因为调用了invalidate().因此请尽量减少调用invaildate()的次数。如果可能的话，尽量调用含有4个参数的invalidate()方法而不是没有参数的invalidate()。没有参数的invalidate会强制重绘整个view。

另外一个非常耗时的操作是请求layout。任何时候执行requestLayout()，会使得Android UI系统去遍历整个View的层级来计算出每一个view的大小。如果找到有冲突的值，它会需要重新计算好几次。另外需要尽量保持View的层级是扁平化的，这样对提高效率很有帮助。

如果你有一个复杂的UI，你应该考虑写一个自定义的ViewGroup来执行他的layout操作。与内置的view不同，自定义的view可以使得程序仅仅测量这一部分，这避免了遍历整个view的层级结构来计算大小。这个PieChart 例子展示了如何继承ViewGroup作为自定义view的一部分。PieChart 有子views，但是它从来不测量它们。而是根据他自身的layout法则，直接设置它们的大小。

# Layout 布局

## Android中常用的五种布局

* FrameLayout（框架布局）
* LinearLayout （线性布局）
* AbsoluteLayout（绝对布局）
* RelativeLayout（相对布局）
* TableLayout（表格布局）

## LinearLayout和RelativeLayout性能对比

1.RelativeLayout会让子View调用2次onMeasure，LinearLayout 在有weight时，也会调用子View2次onMeasure
2.RelativeLayout的子View如果高度和RelativeLayout不同，则会引发效率问题，当子View很复杂时，这个问题会更加严重。如果可以，尽量使用padding代替margin。
3.在不影响层级深度的情况下,使用LinearLayout和FrameLayout而不是RelativeLayout。

最后再思考一下文章开头那个矛盾的问题，为什么Google给开发者默认新建了个RelativeLayout，而自己却在DecorView中用了个LinearLayout。因为DecorView的层级深度是已知而且固定的，上面一个标题栏，下面一个内容栏。采用RelativeLayout并不会降低层级深度，所以此时在根节点上用LinearLayout是效率最高的。而之所以给开发者默认新建了个RelativeLayout是希望开发者能采用尽量少的View层级来表达布局以实现性能最优，因为复杂的View嵌套对性能的影响会更大一些。

参考 ：http://www.jianshu.com/p/8a7d059da746

## Android中px,sp,dip,dp的区别与联系

## Asset目录与res目录的区别。

res 目录下面有很多文件，例如 drawable,mipmap,raw 等。res 下面除了 raw 
文件不会被压缩外，其余文件都会被压缩。同时 res目录下的文件可以通过R 文件访问。Asset 也是用来存储资源，但是 asset 
文件内容只能通过路径或者 AssetManager 读取。 [官方文档](https://developer.android.com/studio/projects/index.html)

# 动画

## Android属性动画特性

Android 起初有两种动画：Frame Animation（逐帧动画） Tween Animation（补间动画）,但是在用的时候发现这两种动画有时候并不能满足我们的一些需要，所以Google在Androi3.0的时候推出了（Property Animation）属性动画,至于为什么前边的两种动画不能满足我们的需要，请往下看：

Frame Animation(逐帧动画)

逐帧动画就是UI设计多张图片组成一个动画，然后将它们组合链接起来进行动画播放。该方式类似于早期电影的制作原理：具体实现方式就不多说了，你只需要让你们的UI出多张图片，然后你顺序的组合就可以（前提是UI给您做图）

Tween Animation(补间动画) 

 Tween Animation：是对某个View进行一系列的动画的操作，包括淡入淡出（Alpha），缩放（Scale），平移（Translate），旋转（Rotate）四种模式

Tween Animation(补间动画)的一些缺点：

1.Tween Animation（补间动画）只是针对于View，超脱了View就无法操作了，这句话的意思是：假如我们需要对一个Button，ImageView，LinearLayout或者是其他的继承自View的各种组件进行动画的操作时，Tween Animation是可以帮我们完成我们需要完成的功能的，但是如果我们需要用到对一个非View的对象进行动画操作的话，那么补间动画就没办法实现了。举个例子：比如我们有一个自定义的View，在这个View中有一个Point对象用于管理坐标，然后在onDraw()方法中的坐标就是根据该Pointde坐标值进行绘制的。也就是说，如果我们可以对Point对象进行动画操作，那么整个自定义的View，那么整个自继承View的当前类就都有了动画，但是我们的目的是不想让View有动画，只是对动画中的Point坐标产生动画，这样补间动画就不能满足了。
2.Tween Animation动画有四种动画操作（移动，缩放，旋转，淡入淡出），但是我们现在有个需求就是将当前View的背景色进行改变呢？抱歉Tween Animation是不能帮助我们实现的。
3.Tween Animation动画只是改变View的显示效果而已，但是不会真正的去改变View的属性，举个例子：我们现在屏幕的顶部有一个小球，然后通过补间动画让他移动到右下角，然后我们给这个小球添加了点击事件，希望位置移动到右下角的时候点击小球能的放大小球。但是点击事件是绝对不会触发的，原因是补间动画只是将该小球绘制到了屏幕的右下角，实际这个小球还是停在屏幕的顶部，所以你在右下角点击是没有任何反应的。

Property Animatior(属性动画)

属性动画是Android3.0之后引进的，它更改的是动画的实际属性，在Tween Animation(补间动画)中，其改变的是View的绘制效果，真正的View的属性是改变不了的，比如你将你的Button位置移动之后你再次点击Button是没有任何点击效果的，或者是你如何缩放你的Button大小，缩放后的有效的点击区域还是只有你当初初始的Button的大小的点击区域，其位置和大小的属性并没有改变。而在Property Animator(属性动画)中，改变的是动画的实际属性，如Button的缩放，Button的位置和大小属性值都会发生改变。而且Property Animation不止可以应用于View，还可以应用于任何对象，Property Animation只是表示一个值在一段时间内的改变，当值改变时要做什么事情完全是你自己决定的。

## Android动画框架实现原理

Animation 框架定义了透明度，旋转，缩放和位移几种常见的动画，而且控制的是整个View。实现原理：

每次绘制视图时，View 所在的 ViewGroup 中的 drawChild 函数获取该View 的 Animation 的 Transformation 值，然后调用canvas.concat(transformToApply.getMatrix())，通过矩阵运算完成动画帧，如果动画没有完成，继续调用 invalidate() 函数，启动下次绘制来驱动动画，动画过程中的帧之间间隙时间是绘制函数所消耗的时间，可能会导致动画消耗比较多的CPU资源，最重要的是，动画改变的只是显示，并不能相应事件。

参考：http://www.tuicool.com/articles/bYfYVv

# 图片缓存

## 图片三级缓存实现？自己设计一个图片加载框架

参考：http://www.jianshu.com/p/2cd59a79ed4a

参考：http://www.csdn.net/article/2015-10-21/2825984 — Android 图片缓存开源库

## 对 LruCache 的理解

## **Bitmap**的分析与使用

参考：

[Android 从具体实例分析Bitmap使用时候内存注意点](http://blog.csdn.net/wuyuxing24/article/details/51675133)

[Android之Bitmap大图加载处理](http://blog.csdn.net/xu_fu/article/details/8262153)

[Loading Large Bitmaps Efficiently](https://developer.android.com/training/displaying-bitmaps/load-bitmap.html)

## Bitmap 压缩

```java
public static Bitmap create(byte[] bytes, int maxWidth, int maxHeight) {
​		//上面的省略了
​        option.inJustDecodeBounds = true;
​        BitmapFactory.decodeByteArray(bytes, 0, bytes.length, option);
​        int actualWidth = option.outWidth;
​        int actualHeight = option.outHeight;

​        // 计算出图片应该显示的宽高
​        int desiredWidth = getResizedDimension(maxWidth, maxHeight, actualWidth, actualHeight);
​        int desiredHeight = getResizedDimension(maxHeight, maxWidth, actualHeight, actualWidth);

​        option.inJustDecodeBounds = false;
​        option.inSampleSize = findBestSampleSize(actualWidth, actualHeight,
​                desiredWidth, desiredHeight);
​        Bitmap tempBitmap = BitmapFactory.decodeByteArray(bytes, 0, bytes.length, option);

​        // 做缩放
​        if (tempBitmap != null
​                && (tempBitmap.getWidth() > desiredWidth || tempBitmap
​                .getHeight() > desiredHeight)) {
​            bitmap = Bitmap.createScaledBitmap(tempBitmap, desiredWidth,
​                    desiredHeight, true);
​            tempBitmap.recycle();
​        } else {
​            bitmap = tempBitmap;
​        }
​    }

​    return bitmap;
}
```

你这么做，decodeByteArray两次不是更占内存吗？😂😂😂 

第一次设置inJustDecodeBounds = true 时候是不占内存的，因为返回的是null

一脸不相信我的说：噢，这地方我下去再看看。

吓得我回来了以后赶紧又看了看，还好没有记错，见源码注释

```java
/**
* If set to true, the decoder will return null (no bitmap), but
* the out... fields will still be set, allowing the caller to query
* the bitmap without having to allocate the memory for its pixels.
    */
   public boolean inJustDecodeBounds;
```

## ARGB_8888占用内存大小

首先说说本题的答案，是4byte，即ARGB各占用8个比特来描述。详细解答看这里[你的 Bitmap 究竟占多大内存](http://bugly.qq.com/bbs/forum.php?mod=viewthread&tid=498) 

1 byte = 8 bit

8+8+8+8 = 32    32/8 = 4 byte 一个像素就占4byte

## 如何判断本地缓存的时候数据需要从网络端获取



# ListView

## ListView的优化

## ListView卡顿原因

1. 在adapter中的getView方法中尽量少使用逻辑
2. 尽最大可能避免GC
3. 滑动的时候不加载图片
4. 将ListView的scrollingCache和animateCache设置为false
5. item的布局层级越少越好
6. 使用ViewHolder

## ListView 的实现原理

参考：http://blog.csdn.net/guolin_blog/article/details/44996879

## ViewHolder



## ListView 下拉刷新、上拉加载更多实现原理

参考：http://blog.csdn.net/zhangphil/article/details/47036177

https://github.com/Aspsine/IRecyclerView — 一个开源库

## RecyclerView和ListView的异同

参考：http://www.tuicool.com/articles/aeeaQ3J

http://blog.csdn.net/sanjay_f/article/details/48830311

- RecyclerView 自带 ViewHolder；而 ListView 则需要自定义。
- RecyclerView 支持水平和垂直滚动；而 ListView 只支持垂直滚动。
- RecyclerView 提供默认的列表项动画实现，例如：添加、删除和移动列表项动画。
- ListView通过AdapterView.OnItemClickListener接口来探测点击事件。而RecyclerView则通过
  RecyclerView.OnItemTouchListener接口来探测触摸事件。它虽然增加了实现的难度，但是却给予开发人员拦截触摸事件更多的
  控制权限。
- ListView可以设置选择模式，并添加MultiChoiceModeListener；而 RecyclerView 没有该功能。

# 容器

## SparseArray 和 HashMap 的区别

# 内存相关

## 什么情况会导致内存泄漏

参考：

[Android内存泄漏总结](https://github.com/GeniusVJR/LearningNotes/blob/master/Part1/Android/Android%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F%E6%80%BB%E7%BB%93.md)

[Handler内存泄漏分析及解决](http://www.jianshu.com/p/cb9b4b71a820)

内存泄漏，简单点说就是该被释放的对象没有释放，一直被某个或某些实例所持有却不再被使用导致 GC 不能回收。

内存泄漏是指无用对象（不再使用的对象）持续占有内存或无用对象的内存得不到及时释放，从而造成内存空间的浪费称为内存泄漏。

1. 资源对象没关闭造成的内存泄漏。对于使用了BraodcastReceiver，ContentObserver，File，游标 Cursor，Stream，Bitmap等资源的使用，应该在Activity销毁时及时关闭或者注销，否则这些资源将不会被回收，造成内存泄漏。
2. 构造Adapter时，没有使用缓存的convertView。
3. Bitmap对象不在使用时没有调用recycle()释放内存。
4. 长生命周期持有短生命周期对象的引用造成的内存泄漏。试着使用关于application的context来替代和activity相关的context。保持对对象生命周期的敏感，特别注意单例、静态对象、全局性集合等的生命周期。
5. 注册没取消造成的内存泄漏。
6. 集合中对象没清理造成的内存泄漏。集合类如果仅仅有添加元素的方法，而没有相应的删除机制，导致内存被占用。如果这个集合类是全局性的变量 (比如类中的静态属性，全局性的 map 等即有静态引用或 final 一直指向它)，那么没有相应的删除机制，很可能导致集合所占用的内存只增不减。
7. 单例造成的内存泄漏。由于单例的静态特性使得其生命周期跟应用的生命周期一样长，所以如果使用不恰当的话，很容易造成内存泄漏。
8. 匿名内部类和非静态内部类持有外部类的引用造成的内存泄漏。
9. Handler 造成的内存泄漏。

- 资源对象没关闭造成的内存泄漏。

描述： 资源性对象比如(Cursor，File文件等)往往都用了一些缓冲，我们在不使用的时候，应该及时关闭它们，以便它们的缓冲及时回收内存。它们的缓冲不仅存在于 java虚拟机内，还存在于java虚拟机外。如果我们仅仅是把它的引用设置为null,而不关闭它们，往往会造成内存泄漏。因为有些资源性对象，比如 SQLiteCursor(在析构函数finalize(),如果我们没有关闭它，它自己会调close()关闭)，如果我们没有关闭它，系统在回收它时也会关闭它，但是这样的效率太低了。因此对于资源性对象在不使用的时候，应该调用它的close()函数，将其关闭掉，然后才置为null.在我们的程序退出时一定要确保我们的资源性对象已经关闭。 程序中经常会进行查询数据库的操作，但是经常会有使用完毕Cursor后没有关闭的情况。如果我们的查询结果集比较小，对内存的消耗不容易被发现，只有在常时间大量操作的情况下才会复现内存问题，这样就会给以后的测试和问题排查带来困难和风险。

2. 构造Adapter时，没有使用缓存的convertView

描述： 以构造ListView的BaseAdapter为例，在BaseAdapter中提供了方法： public View getView(int position, ViewconvertView, ViewGroup parent) 来向ListView提供每一个item所需要的view对象。初始时ListView会从BaseAdapter中根据当前的屏幕布局实例化一定数量的 view对象，同时ListView会将这些view对象缓存起来。当向上滚动ListView时，原先位于最上面的list item的view对象会被回收，然后被用来构造新出现的最下面的list item。这个构造过程就是由getView()方法完成的，getView()的第二个形参View convertView就是被缓存起来的list item的view对象(初始化时缓存中没有view对象则convertView是null)。由此可以看出，如果我们不去使用 convertView，而是每次都在getView()中重新实例化一个View对象的话，即浪费资源也浪费时间，也会使得内存占用越来越大。 ListView回收list item的view对象的过程可以查看: android.widget.AbsListView.java --> voidaddScrapView(View scrap) 方法。 示例代码：

```
public View getView(int position, ViewconvertView, ViewGroup parent) {
View view = new Xxx(...); 
... ... 
return view; 
} 
```

修正示例代码：

```
public View getView(int position, ViewconvertView, ViewGroup parent) {
View view = null; 
if (convertView != null) { 
view = convertView; 
populate(view, getItem(position)); 
... 
} else { 
view = new Xxx(...); 
... 
} 
return view; 
} 
```

1. Bitmap对象不在使用时调用recycle()释放内存

描述： 有时我们会手工的操作Bitmap对象，如果一个Bitmap对象比较占内存，当它不在被使用的时候，可以调用Bitmap.recycle()方法回收此对象的像素所占用的内存，但这不是必须的，视情况而定。

1. 试着使用关于application的context来替代和activity相关的context

这是一个很隐晦的内存泄漏的情况。有一种简单的方法来避免context相关的内存泄漏。最显著地一个是避免context逃出他自己的范围之外。使用Application context。这个context的生存周期和你的应用的生存周期一样长，而不是取决于activity的生存周期。如果你想保持一个长期生存的对象，并且这个对象需要一个context,记得使用application对象。你可以通过调用 Context.getApplicationContext() or Activity.getApplication()来获得。更多的请看这篇文章如何避免 Android内存泄漏。

1. 注册没取消造成的内存泄漏

一些Android程序可能引用我们的Anroid程序的对象(比如注册机制)。即使我们的Android程序已经结束了，但是别的引用程序仍然还有对我们的Android程序的某个对象的引用，泄漏的内存依然不能被垃圾回收。调用registerReceiver后未调用unregisterReceiver。 比如:假设我们希望在锁屏界面(LockScreen)中，监听系统中的电话服务以获取一些信息(如信号强度等)，则可以在LockScreen中定义一个 PhoneStateListener的对象，同时将它注册到TelephonyManager服务中。对于LockScreen对象，当需要显示锁屏界面的时候就会创建一个LockScreen对象，而当锁屏界面消失的时候LockScreen对象就会被释放掉。 但是如果在释放 LockScreen对象的时候忘记取消我们之前注册的PhoneStateListener对象，则会导致LockScreen无法被垃圾回收。如果不断的使锁屏界面显示和消失，则最终会由于大量的LockScreen对象没有办法被回收而引起OutOfMemory,使得system_process 进程挂掉。 虽然有些系统程序，它本身好像是可以自动取消注册的(当然不及时)，但是我们还是应该在我们的程序中明确的取消注册，程序结束时应该把所有的注册都取消掉。

1. 集合中对象没清理造成的内存泄漏

我们通常把一些对象的引用加入到了集合中，当我们不需要该对象时，并没有把它的引用从集合中清理掉，这样这个集合就会越来越大。如果这个集合是static的话，那情况就更严重了。

## 什么情况会导致 OOM

参考：http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0920/3478.html

如果避免 OOM：

- 减少对象的内存占用

1. 使用更加轻量的数据结构。例如，我们可以考虑使用 ArrayMap 或 SparseArray 而不是 HashMap 等传统数据结构。
2. 避免在 Android 里面使用 Enum。
3. 减小 Bitmap 对象的内存占用。缩放比例，解码格式。
4. 使用更小的图片。

- 内存对象的重复利用

1. 复用系统自带的资源。但需要留意不同系统版本的差异性。
2. 在 ListView 和 GridView 等大量出现重复子组件的视图里对 ConvertView 复用。
3. Bitmap 对象的复用。在  ListView 和 GridView 等显示大量图片的控件里，需要使用 LRU 的机制来缓存处理好的 Bitmap。
4. 避免在 onDraw 方法里面执行对象的创建。
5. 使用大量字符串拼接操作是，考虑使用 StringBuffer 代替 “+”。

- 避免对象的内存泄漏

1. 注意 Activity 的泄漏。1）内部类引用导致 Activity 的泄漏。典型的是 Handler 导致的泄漏。为了解决这个问题，可以在UI退出之前，执行remove Handler消息队列中的消息与runnable对象。或者是使用Static + WeakReference的方式来达到断开Handler与Activity之间存在引用关系的目的。2）Activity Context被传递到其他实例中，这可能导致自身被引用而发生泄漏。
2. 考虑使用 Application Context 而不是 Activity Context。对于大部分非必须使用Activity Context的情况（Dialog的Context就必须是Activity Context），我们都可以考虑使用Application Context而不是Activity的Context，这样可以避免不经意的Activity泄露。
3. 注意临时Bitmap对象的及时回收。
4. 注意监听器的注销。
5. 注意缓存容器中的对象泄漏。
6. 注意 WebView 的泄漏。
7. 注意 Cursor 对象是否及时关闭。


## Android 为每个应用程序分配的内存大小是多少

android 程序内存一般限制在16M，也有的是24M

## 查看每个应用程序最高可用内存

```java
    int maxMemory = (int) (Runtime.getRuntime().maxMemory() / 1024);  
    Log.d("TAG", "Max memory is " + maxMemory + "KB");  
```

## Android中弱引用与软引用的应用场景。

# ANR

## ANR 定位和修正

如果开发机器上出现问题，我们可以通过查看/data/anr/traces.txt即可，最新的ANR信息在最开始部分。

出现 ANR 原因：

- 主线程被IO操作（从4.0之后网络IO不允许在主线程中）阻塞。
- 主线程中存在耗时的计算。
- 主线程中错误的操作，比如Thread.wait或者Thread.sleep等。
- 应用在5秒内未响应用户的输入事件（如按键或者触摸）
- BroadcastReceiver未在10秒内完成相关的处理。
- Service在特定的时间内无法处理完成 20秒

修正方法：

- 使用AsyncTask处理耗时IO操作。

- 使用Thread或者HandlerThread时，调用Process.setThreadPriority(Process.THREAD_PRIORITY_BACKGROUND)设置优先级，否则仍然会降低程序响应，因为默认Thread的优先级和主线程相同。

- 使用Handler处理工作线程结果，而不是使用Thread.wait()或者Thread.sleep()来阻塞主线程。

- Activity的onCreate和onResume回调中尽量避免耗时的代码。

- BroadcastReceiver中onReceive代码也要尽量减少耗时，建议使用IntentService处理

如何避免：
  1. UI线程尽量只做跟UI相关的工作
  2. 耗时的操作(比如数据库操作，I/O，连接网络或者别的有可能阻塞UI线程的操作)把它放在单独的线程处理
  3. 尽量用Handler来处理UIThread和别的Thread之间的交互
  4. 解决的逻辑。使用 AsyncTask 时：在doInBackground()方法中执行耗时操作；在onPostExecuted()更新UI 。使用Handler实现异步任务时：在子线程中处理耗时操作，处理完成之后，通过handler.sendMessage()传递处理结果；在handler的handleMessage()方法中更新 UI 或者使用handler.post()方法将消息放到Looper中。

# 性能优化

## 怎么对 Android APP 进行性能优化

参考：[Android 性能优化](https://github.com/GeniusVJR/LearningNotes/blob/master/Part1/Android/Android%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96.md)

## Android APP 内存分析工具有哪些

## 屏幕适配经验



# 数据存储

## 数据存储，数据持久化的方式有哪些

- SharedPreference
- 数据库SQLite
- 文件
- ContentProvider内容提供者，如联系人
- 网络
- SQLite：SQLite是一个轻量级的数据库，支持基本的SQL语法，是常被采用的一种数据存储方式。Android为此数据库提供了一个名为SQLiteDatabase的类，封装了一些操作数据库的api
- SharedPreference： 除SQLite数据库外，另一种常用的数据存储方式，其本质就是一个xml文件，常用于存储较简单的参数设置。
- File： 即常说的文件（I/O）存储方法，常用语存储大数量的数据，但是缺点是更新数据将是一件困难的事情。
- ContentProvider: Android系统中能实现所有应用程序共享的一种数据存储方式，由于数据通常在各应用间的是互相私密的，所以此存储方式较少使用，但是其又是必不可少的一种存储方式。例如音频，视频，图片和通讯录，一般都可以采用此种方式进行存储。每个Content Provider都会对外提供一个公共的URI（包装成Uri对象），如果应用程序有数据需要共享时，就需要使用Content Provider为这些数据定义一个URI，然后其他的应用程序就通过Content Provider传入这个URI来对数据进行操作。

## 文件和数据库哪个效率高



## SharedPreference 实现

## 如何导入外部数据库

把数据库存放到 res/raw 目录下，然后在第一次安装启动应用的时间把该数据库拷贝到应用的内部存储空间即 android 系统下的 /data/data/packagename/ 目录下。



# 网络通讯

## 描述一次网络请求的流程

1. 建立TCP连接 在HTTP工作开始之前，Web浏览器首先要通过网络与Web服务器建立连接，该连接是通过TCP来完成的，该协议与IP协议共同构建Internet，即著名的TCP/IP协议族，因此Internet又被称作是TCP/IP网络。HTTP是比TCP更高层次的应用层协议，根据规则，只有低层协议建立之后才能进行更高层协议的连接，因此，首先要建立TCP连接，一般TCP连接的端口号是80。
2. Web浏览器向Web服务器发送请求命令 一旦建立了TCP连接，Web浏览器就会向Web服务器发送请求命令。例如：GET/sample/hello.jsp HTTP/1.1。
3. Web浏览器发送请求头信息 浏览器发送其请求命令之后，还要以头信息的形式向Web服务器发送一些别的信息，之后浏览器发送了一空白行来通知服务器，它已经结束了该头信息的发送。
4. Web服务器应答 客户机向服务器发出请求后，服务器会客户机回送应答， HTTP/1.1 200 OK ，应答的第一部分是协议的版本号和应答状态码。
5. Web服务器发送应答头信息 正如客户端会随同请求发送关于自身的信息一样，服务器也会随同应答向用户发送关于它自己的数据及被请求的文档。
6. Web服务器向浏览器发送数据 Web服务器向浏览器发送头信息后，它会发送一个空白行来表示头信息的发送到此为结束，接着，它就以Content-Type应答头信息所描述的格式发送用户所请求的实际数据。
7. Web服务器关闭TCP连接 一般情况下，一旦Web服务器向浏览器发送了请求数据，它就要关闭TCP连接，然后如果浏览器或者服务器在其头信息加入了这行代码：Connection:keep-alive

TCP连接在发送后将仍然保持打开状态，于是，浏览器可以继续通过相同的连接发送请求。保持连接节省了为每个请求建立新连接所需的时间，还节约了网络带宽。

## 推送心跳包是TCP包还是UDP包或者HTTP包

其实聊起这个问题是因为最近看到 [@睡不着起不来的万宵](http://weibo.com/u/2951317192) 同学写的一篇文章《[Android推送技术研究](http://www.jianshu.com/p/584707554ed7)》结果就产生了这个没回答出来的问题(妈蛋，自己给自己挖坑 - -)
最后看了这篇文章(好像是转的，没找到原地址)[android 心跳的分析](http://blog.csdn.net/wangliang198901/article/details/16542567)
原来心跳包的实现是调用了`socket.sendUrgentData(0xFF)`这句代码实现的，所以，当然是TCP包。

## Android 代码中实现 WAP 方式联网

-通过 APN 列表，获取代码服务器和端口号，如果未设置，则设置成对应运营商的配置。
-实现 HtppClient 代理。

## 断点续传



# 动态加载

## 插件化，动态加载



## Android中ClassLoader和java中有什么关系和区别？



# JNI

## JNI开发流程



# WebView

## WebView和JS



# jar

## 65k限制 做内部库设计时，最重要的考虑是jar的成本，方法数、体积。



# 其他

## APP启动过程



## 如何判断应用被强杀

在Applicatio中定义一个static常量，赋值为－1，在欢迎界面改为0，如果被强杀，application重新初始化，在父类Activity判断该常量的值。

## 应用被强杀如何解决

如果在每一个Activity的onCreate里判断是否被强杀，冗余了，封装到Activity的父类中，如果被强杀，跳转回主界面，如果没有被强杀，执行Activity的初始化操作，给主界面传递intent参数，主界面会调用onNewIntent方法，在onNewIntent跳转到欢迎页面，重新来一遍流程。

## 简述静默安装的原理，如何在无需root权限的情况下实现静默安装？

## Serializable和Parcelable的区别

- 都能实现序列化且可用于Intent间的数据传递
- Serializable是Java中的序列化接口，使用简单但开销大，序列化和反序列化过程需要大量I/O操作。
- Parcelable更适合Android平台，使用麻烦但效率高，主要用在内存序列化上。



## Debug和Release状态的不同



## Toolbar的使用



## 低版本 SDK 如何实现高版本 API

自己实现或@TargetApi annotation

在低版本的 SDK 使用高版本的 API 会报错。解决方法是：在高版本 SDK 中使用高版本 API，低版本 SDK 中自己实现。

- 在使用了高版本 API 的方法前面加一个 @TargetApi(API版本号)。
- 在代码中判断版本号来控制不同的版本使用不同的代码。

```
@TargetApi(11) 
public void text() { 
if(Build.VERSION.SDK_INT >= 11){ 
	// 使用 API 11 的方法
} else {
	// 使用自己实现的方法
}
```

## 实现一个单例

```
public class Singleton{
private volatile static Singleton mSingleton;
private Singleton(){
}
public static Singleton getInstance(){
  if(mSingleton == null){\\A
    synchronized(Singleton.class){\\C
     if(mSingleton == null)
      mSingleton = new Singleton();\\B
      }
    }
    return mSingleton;
  }
}
```

## 如 View 的事件分发，屏幕适配经验，性能优化的经验、Java 线程几种用法等    



## 如 AIDL、插件化, 如网络的优化, 如缓存的处理, 如插件化, 如 Service 保活

# 设计模式

## Android 中主要用到的几种设计模式



## Android设计模式

参考：http://blog.csdn.net/bboyfeiyu/article/details/44563871

# 架构设计


## mvc mvp mvvm

参考：http://www.tianmaying.com/tutorial/AndroidMVC

![](http://o9sn2y8lr.bkt.clouddn.com/16-10-19/71303594.jpg)

# Volley 源码解析

参考：

[http://a.codekk.com/detail/Android/grumoon/Volley%20%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90](http://a.codekk.com/detail/Android/grumoon/Volley%20%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90)

### Volley的磁盘缓存

在面试的时候，聊到 Volley 请求到网络的数据缓存。当时说到是 Volley 会将每次通过网络请求到的数据，采用`FileOutputStream`，写入到本地的文件中。 
那么问题来了：这个缓存文件，是声明在一个SD卡文件夹中的(也可以是getCacheFile())。如果不停的请求网络数据，这个缓存文件夹将无限制的增大，最终达到SD卡容量时，会发生无法写入的异常(因为存储空间满了)。
这个问题的确以前没有想到，当时也没说出怎么回事。回家了赶紧又看了看代码才知道，原来 Volley 考虑过这个问题(汗!想想也是)
翻看代码`DiskBasedCache#pruneIfNeeded()`

```
private void pruneIfNeeded(int neededSpace) {
    if ((mTotalSize + neededSpace) < mMaxCacheSizeInBytes) {
        return;
    }
    
    long before = mTotalSize;
    int prunedFiles = 0;
    long startTime = SystemClock.elapsedRealtime();

    Iterator<Map.Entry<String, CacheHeader>> iterator = mEntries.entrySet().iterator();
    while (iterator.hasNext()) {
        Map.Entry<String, CacheHeader> entry = iterator.next();
        CacheHeader e = entry.getValue();
        boolean deleted = getFileForKey(e.key).delete();
        if (deleted) {
            mTotalSize -= e.size;
        } else {
	//print log
        }
        iterator.remove();
        prunedFiles++;
        if ((mTotalSize + neededSpace) < mMaxCacheSizeInBytes * HYSTERESIS_FACTOR) {
            break;
        }
    }
}
```

其中`mMaxCacheSizeInBytes`是构造方法传入的一个缓存文件夹的大小，如果不传默认是5M的大小。
通过这个方法可以发现，每当被调用时会传入一个`neededSpace`，也就是需要申请的磁盘大小(即要新缓存的那个文件所需大小)。首先会判断如果这个`neededSpace`申请成功以后是否会超过最大可用容量，如果会超过，则通过遍历本地已经保存的缓存文件的header(header中包含了缓存文件的缓存有效期、占用大小等信息)去删除文件，直到可用容量不大于声明的缓存文件夹的大小。 
其中`HYSTERESIS_FACTOR`是一个值为0.9的常量，应该是为了防止误差的存在吧(我猜的)。

### Volley缓存命中率的优化

如果让你去设计Volley的缓存功能，你要如何增大它的命中率。
可惜了，如果上面的缓存功能是昨天看的，今天的面试这个问题就能说出来了。
还是上面的代码，在缓存内容可能超过缓存文件夹的大小时，删除的逻辑是直接遍历header删除。这个时候删除的文件有可能是我们上一次请求时刚刚保存下来的，屁股都还没坐稳呢，现在立即删掉，有点舍不得啊。
如果遍历的时候，判断一下，首先删除超过缓存有效期的(过期缓存)，其次按照LRU算法，删除最久未使用的，岂不是更合适？

### Volley缓存文件名的计算

这个是我一直没弄懂的问题。
如下代码：

```
private String getFilenameForKey(String key) {
    int firstHalfLength = key.length() / 2;
    String localFilename = String.valueOf(key.substring(0, firstHalfLength).hashCode());
    localFilename += String.valueOf(key.substring(firstHalfLength).hashCode());
    return localFilename;
}
```

为什么会要把一个key分成两部分，分别求hashCode，最后又做拼接。
这个问题之前在stackoverflow上问过 [#链接](http://stackoverflow.com/questions/34984302/why-volley-diskbasedcache-splicing-without-direct-access-to-the-cache-file-name/34987423#34987423) 
原谅我，别人的回答我最初并没有看懂。直到最近被问到，如果让你设计一个HashMap，如何避免value被覆盖，我才想到原因。
先来看一下 `String#hashCode()` 的实现：

```
@Override public int hashCode() {
    int hash = hashCode;
    if (hash == 0) {
        if (count == 0) {
            return 0;
        }
        final int end = count + offset;
        final char[] chars = value;
        for (int i = offset; i < end; ++i) {
            hash = 31*hash + chars[i];
        }
        hashCode = hash;
    }
    return hash;
}
```

从上面的实现可以看到，String的hashcode是根据字符数组中每个位置的字母的int值再加上上次hash值乘以31，这种算法求出来的，至于为什么是31，我也不清楚。
但是可以肯定一点，hashcode并不是唯一的。不信你运行下面这两个输出：

```
System.out.print("======" + "vFrKiaNHfF7t[9::E[XsX?L7xPp3DZSteIZvdRT8CX:w6d;v<_KZnhsM_^dqoppe".hashCode());
System.out.print("======" + "hI4pFxGOfS@suhVUd:mTo_begImJPB@Fl[6WJ?ai=RXfIx^=Aix@9M;;?Vdj_Zsi".hashCode());
```

这两个字符串是根据hashcode的算法逆向出来的，他们的hashcode都是12345。逆向算法请见[这里](http://my.oschina.net/backtract/blog/169310)
再回到我们的问题，为什么会要把一个key分成两部分。现在可以肯定的答出，目的是为了尽可能避免hashcode重复造成的文件名重复(求两次hash两次都与另一个url重复的概率总要比一次重复的概率小吧)。
顺带再提一点，就像上面说的，概率小并不代表不存在。但是Java计算hashcode的速度是很快的，应该是在效率和安全性上取舍的结果吧。

# Glide源码解析

参考：

[http://www.lightskystreet.com/2015/10/12/glide_source_analysis/](http://www.lightskystreet.com/2015/10/12/glide_source_analysis/)

[http://frodoking.github.io/2015/10/10/android-glide/](http://frodoking.github.io/2015/10/10/android-glide/)

# Retrofit源码分析

参考：

[Retrofit源码分析](http://www.jianshu.com/p/c1a3a881a144)

# EventBus源码分析

参考：

[EventBus源码分析](http://p.codekk.com/blogs/detail/54cfab086c4761e5001b2538)

#  greenDAO

参考：

[Android ORM 框架之 greenDAO 使用心得](http://www.open-open.com/lib/view/open1438065400878.html)

# RxJava

参考：

[RxJava](http://gank.io/post/560e15be2dca930e00da1083)