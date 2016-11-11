---
title: Android 面试：Android 简述题
date: 2016-10-26 11:06:15
categories:
- Android 面试
tags:
- Android 面试
---

本文收集整理了 Android 面试中会遇到与 Android 知识相关的简述题。<!--more-->

# 基本概念

## Android 的四大组件

Acitivity、Service、BroadcastReceiver、ContentProvider

Activity :

应用程序中，一个Activity通常就是一个单独的屏幕，它上面可以显示一些控件也可以监听并处理用户的事件做出响应。

BroadcastReceiver广播接收器:

应用程序可以使用它对外部事件进行过滤只对感兴趣的外部事件(如当电话呼入时，或者数据网络可用时)进行接收并做出响应。广播接收器没有用户界面。然而，它们可以启动一个activity或serice 来响应它们收到的信息，或者用NotificationManager 来通知用户。通知可以用很多种方式来吸引用户的注意力──闪动背灯、震动、播放声音等。一般来说是在状态栏上放一个持久的图标，用户可以打开它并获取消息。

Service 服务:

一个Service 是一段长生命周期的，没有用户界面的程序，可以用来开发如监控类程序。

Content Provider内容提供者 :

android平台提供了Content Provider使一个应用程序的指定数据集提供给其他应用程序。这些数据可以存储在文件系统中、在一个SQLite数据库、或以任何其他合理的方式。其他应用可以通过ContentResolver类(见ContentProviderAccessApp例子)从该内容提供者中获取或存入数据.(相当于在应用外包了一层壳),只有需要在多个应用程序间共享数据是才需要内容提供者。例如，通讯录数据被多个应用程序使用，且必须存储在一个内容提供者中。它的好处:统一数据访问方式。

参考：

[Android四大基本组件介绍与生命周期](http://www.cnblogs.com/bravestarrhu/archive/2012/05/02/2479461.html)

## 四大组件的具体作用以及用法

- Acitivity 用于显示界面，接收用户输入，和用户交互。
- Service 运行于后台无界面的程序，用于在后台完成一下任务，例如：音乐播放等。
- BroadCast Receiver 接收系统或应用发出的广播并作出响应，例如：电话的呼入呼出等。
- Content Provider 用于把APP本身的数据共享给其他APP，提供本APP数据的存取接口给其他APP。

## Android平台的framework的层次结构？

应用层、应用框架层、中间件（核心库和运行时）、Linux内核

# Activity

## Activity 生命周期

![](http://o9sn2y8lr.bkt.clouddn.com/16-10-25/30416013.jpg)

- 启动Activity：onCreate->onStart->onResume
- 锁屏或被其它Activity覆盖：onPause->onStop
- 解锁或由被覆盖状态再回到前台：onRestart->onStart->onResume
- 跳转到其它Activity或按Home进入后台：onPause->onStop
- 退回到此Activity：onRestart->onStart->onResume
- 退出此Activity：onPause->onStop->onDestory
- 对话框弹出不会执行任何生命周期(注：对话框如果是Activity(Theme为Dialog)，还是会执行生命周期的)
- 从A跳转到B：当B的主题为透明时，A只会执行onPause（A-onPause->B-(onCreate->onStart->onResume)）
- 从A跳转到B：A-onPause->B-(onCreate->onStart->onResume)-A-onStop(注意是A执行onPause后开始执行B的生命周期，B执行onResume后，A才执行onStop，所以尽量不要在onPause中做耗时操作)
- 从B返回到A：B-onPause->A-(onRestart->onStart->onResume)-B-(onStop->onDestroy)

## Activity和Fragment生命周期有哪些？
Activity——onCreate->onStart->onResume->onPause->onStop->onDestroy

Fragment——onAttach->onCreate->onCreateView->onActivityCreated->onStart->onResume->onPause->onStop->onDestroyView->onDestroy->onDetach

## Activity四种启动模式的区别（LanchMode 的应用场景）

- standard 模式

这是默认模式，每次激活Activity时都会创建Activity实例，并放入任务栈中。

- singleTop 模式

如果在任务的栈顶正好存在该Activity的实例，就重用该实例( 会调用实例的 onNewIntent() )，否则就会创建新的实例并放入栈顶，即使栈中已经存在该Activity的实例，只要不在栈顶，都会创建新的实例。

- singleTask 模式

如果在栈中已经有该Activity的实例，就重用该实例(会调用实例的 onNewIntent() )。重用时，会让该实例回到栈顶，因此在它上面的实例将会被移出栈。如果栈中不存在该实例，将会创建新的实例放入栈中。

- singleInstance 模式

在一个新栈中创建该Activity的实例，并让多个应用共享该栈中的该Activity实例。一旦该模式的Activity实例已经存在于某个栈中，任何应用再激活该Activity时都会重用该栈中的实例( 会调用实例的 onNewIntent() )。其效果相当于多个应用共享一个应用，不管谁激活该 Activity 都会进入同一个应用中。

设置启动模式的位置在 AndroidManifest.xml 文件中 Activity 元素的 Android:launchMode 属性。

singleTop适合接收通知启动的内容显示页面。例如，某个新闻客户端的新闻内容页面，如果收到10个新闻推送，每次都打开一个新闻内容页面是很烦人的。

singleTask适合作为程序入口点。例如浏览器的主界面。不管从多少个应用启动浏览器，只会启动主界面一次，其余情况都会走onNewIntent，并且会清空主界面上面的其他页面。

singleInstance应用场景：闹铃的响铃界面。 你以前设置了一个闹铃：上午6点。在上午5点58分，你启动了闹铃设置界面，并按 Home 键回桌面；在上午5点59分时，你在微信和朋友聊天；在6点时，闹铃响了，并且弹出了一个对话框形式的 Activity(名为 AlarmAlertActivity) 提示你到6点了(这个 Activity 就是以 SingleInstance 加载模式打开的)，你按返回键，回到的是微信的聊天界面，这是因为 AlarmAlertActivity 所在的 Task 的栈只有他一个元素， 因此退出之后这个 Task 的栈空了。如果是以 SingleTask 打开 AlarmAlertActivity，那么当闹铃响了的时候，按返回键应该进入闹铃设置界面。

## Activity中类似onCreate、onStart运用了哪种设计模式，优点是什么

模板模式。每次新建一个Actiivty时都会覆盖onCreate，onStart等方法,这些方法在父类中就相当于一个模板。

## 如何将一个Activity设置成窗口的样式

- 在AndroidManifest.xml文件中设置当前需要改变成窗口样式的Activity的属性。

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
6. ActivityManagerService调用ApplicationThread.scheduleLaunchActivity接口，通知相应的进程执行启动Activity的操作；
7. ApplicationThread把这个启动Activity的操作转发给ActivityThread，ActivityThread通过ClassLoader导入相应的Activity类，然后把它启动起来。



## window和activity之间关系？

## WindowManager 的相关知识

## Activity、Window 和 View 三者的区别

- 一个 Activity 构造的时候一定会构造一个 Window(PhoneWindow)，并且只有一个。
- 这个Window会有一个ViewRoot(View、ViewGroup)。
- 通过addView()加载布局。
- WindowMangerService 接收消息，并且回到 Activity 函数，比如onKeyDown()。

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

- 记录打开的Activity：每打开一个Activity，就记录下来。在需要退出时，关闭每一个Activity即可。
- 发送特定广播：在需要结束应用时，发送一个特定的广播，每个Activity收到广播后，关闭即可。
- 递归退出：在打开新的Activity时使用startActivityForResult，然后自己加标志，在onActivityResult中处理，递归关闭。

## 有几种在activity之间切换的方法？

startActivity()

startActivityForResult()

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

- Fragment可以作为Activity界面的一部分组成出现；
- 可以在一个Activity中同时出现多个Fragment，并且一个Fragment也可以在多个Activity中使用；
- 在Activity运行过程中，可以添加、移除或者替换Fragment；
- Fragment可以响应自己的输入事件，并且有自己的生命周期，它们的生命周期会受宿主Activity的生命周期影响。
- Fragment可以轻松得创建动态灵活的UI设计，可以适应于不同的屏幕尺寸。从手机到平板电脑。
- Fragment 解决Activity间的切换不流畅，轻量切换。
- Fragment 替代TabActivity做导航，性能更好。
- Fragment做局部内容更新更方便，原来为了达到这一点要把多个布局放到一个activity里面，现在可以用多Fragment来代替，只有在需要的时候才加载Fragment，提高性能。

## Fragment嵌套多个Fragment会出现bug吗？

参考：http://blog.csdn.net/megatronkings/article/details/51417510

## 怎么理解Activity和Fragment的关系？

- Fragment 拥有和 Activity 一致的生命周期，它和 Activity 一样被定义为 Controller 层的类。有过中大型项目开发经验的开发者，应该都会遇到过 Activity 过于臃肿的情况，而 Fragment 的出现就是为了缓解这一状况，可以说 它将屏幕分解为多个「Fragment（碎片）」（这句话很重要），但它又不同于 View，它干的实质上就是 Activity 的事情，负责控制 View 以及它们之间的逻辑。
- 将屏幕碎片化为多个 Fragment 后，其实 Activity 只需要花精力去管理当前屏幕内应该显示哪些 Fragments，以及应该对它们进行如何布局就行了。这是一种组件化的思维，用 Fragment 去组合了一系列有关联的 UI 组件，并管理它们之间的逻辑，而 Activity 负责在不同屏幕下（例如横竖屏）布局不同的 Fragments 组合。
- 这种碎片不单单能管理可视的 Views，它也能执行不可视的 Tasks，它提供了 retainInstance 属性，能够在 Activity 因为屏幕状态发生改变（例如切换横竖屏时）而销毁重建时，依然保留实例。这示意着我们能在 RetainedFragment 里面执行一些在屏幕状态发生改变时不被中断的操作。例如使用 RetainedFragment 来缓存在线音乐文件，它在横竖屏切换时依然维持下载进度，并通过一个 DialogFragment 来展示进度。

# Service

## Service的生命周期。

![](http://o9sn2y8lr.bkt.clouddn.com/16-10-25/20789048.jpg)

Service是运行在后台的android组件，没有用户界面，不能与用户交互，可以运行在自己的进程，也可以运行在其他应用程序的上下
文里。

Service随着启动形式的不同，其生命周期稍有差别。当用Context.startService()来启动时，Service的生命周期依次为:oncreate——>onStartCommand——>onDestroy 

当用Context.bindService()启动时:onStart——>onBind——>onUnbind——>onDestroy

## Service有哪些启动方法，有什么区别，怎样停用Service？

Service启动方式有两种；一是Context.startService和Context.bindService。

区别是通过startService启动时Service组件和应用程序没多大的联系；当用访问者启动之后，如果访问者不主动关闭，Service就不会关闭，Service组件之间因为没什么关联，所以Service也不能和应用程序进行数据交互。

而通过bindService进行绑定时，应用程序可以通过ServiceConnection进行数据交互。在实现Service时重写的onBind方法中，其返回的对象会传给ServiceConnection对象的onServiceConnected(ComponentName name, IBinder service)中的service参数；也就是说获取了serivce这个参数就得到了Serivce组件返回的值。Context.bindService(Intent intent,ServiceConnection conn,int flag)其中只要与Service连接成功conn就会调用其onServiceConnected方法

停用Service使用Context.stopService

## 注册Service需要注意什么

无论使用哪种启动方法，都需要在xml里注册你的Service，就像这样:

```java
<service
  android:name=".packnameName.youServiceName"
  android:enabled="true" />
```

## service 可以执行耗时操作吗

不能，超过20s就会出现ARN

## Service和Activity在同一个线程吗

默认情况下是在同一个主线程中。但可以通过配置android:process=":remote" 属性让 Service 运行在不同的进程。

Service分为本地服务（LocalService）和远程服务（RemoteService）：

1、本地服务依附在主进程上而不是独立的进程，这样在一定程度上节约了资源，另外Local服务因为是在同一进程因此不需要IPC，

也不需要AIDL。相应bindService会方便很多。主进程被Kill后，服务便会终止。

2、远程服务为独立的进程，对应进程名格式为所在包名加上你指定的android:process字符串。由于是独立的进程，因此在Activity所在进程被Kill的时候，该服务依然在运行，

不受其他进程影响，有利于为多个进程提供服务具有较高的灵活性。该服务是独立的进程，会占用一定资源，并且使用AIDL进行IPC稍微麻烦一点。

按使用方式可以分为以下三种：

1. startService 启动的服务：主要用于启动一个服务执行后台任务，不进行通信。停止服务使用stopService；
2. bindService 启动的服务：该方法启动的服务可以进行通信。停止服务使用unbindService
3. startService 同时也 bindService 启动的服务：停止服务应同时使用stepService与unbindService

参考：http://www.cnblogs.com/linlf03/p/3296323.html

## Service与Activity怎么实现通信

- 通过Binder对象。Activity调用bindService (Intent service, ServiceConnection conn, int flags)方法，得到Service对象的一个引用，这样Activity可以直接调用到Service中的方法，如果要主动通知Activity，我们可以利用回调方法。Binder 相关知识参考：http://blog.csdn.net/boyupeng/article/details/47011383


- 通过广播。Service向Activity发送消息，可以使用广播，当然Activity要注册相应的接收器。比如Service要向多个Activity发送同样的消息的话，用这种方法就更好。

## Service里面可以弹出 dialog 或 Toast 吗

可以。`getApplicationContext()`

Toast必须在UI主线程上才能正常显示，而在Service中是无法获得Acivity的Context的，在service中想显示出Toast只需将show的消息发送给主线程Looper就可以了。此时便可显示Toast。

```java
Handler handler = new Handler(Looper.getMainLooper());  
handler.post(new Runnable() {  
  public void run() {  
    Toast.makeText(getApplicationContext(), "存Service is runing!",  
                   Toast.LENGTH_SHORT).show();  
  }  
}); 
```

Dialog的用法与此类似，但是要多加个权限，在manifest中添加此权限以弹出dialog 

```java
<uses-permission Android:name="android.permission.SYSTEM_ALERT_WINDOW" />
dialog.getWindow().setType((WindowManager.LayoutParams.TYPE_SYSTEM_ALERT));  
```

## 什么时候使用Service?

比如播放多媒体的时候，用户启动了其他的Activity这个时候程序要在后台继续播放。比如检测SD卡上文件的变化。在或者在后台记录你地理位置的改变等等。

## 说说Activity、Intent、Service是什么关系

一个Activity通常是一个单独的屏幕，每一个Activity都被实现为一个单独的类，这些类都是从Activity基类中继承来的，Activity类显示有视图控件组成的用户接口，并对视图控件的事件做出响应。

Intent的调用是用来进行架构屏幕之间的切换的。Intent是描述应用想要做什么。Intent数据结果中最重要的部分是动作和动作对应的数据，一个动作对应一个动作数据。

Service是运行在后台的代码，不能与用户交互，可以运行在自己的进程，也可以运行在其他应用程序的上下文里。需要通过某一个Activity或其他Context对象来调用。 

Activity 跳转到Activity,Activtiy启动Service,Service打开Activity都需要Intent表明跳转的意图，以及传递参数，Intent是这些组件间信号传递的传承者。

## 怎么在启动一个activity时就启动一个service

在activity的onCreate里写startService(xxx);然后this.finish();结束自己..

这是最简单的方法 可能会有屏幕一闪的现象，如果UI要求严格的话用AIDL把

## 为什么在Service中创建子线程而不是Activity中

这是因为Activity很难对Thread进行控制，当Activity被销毁之后，就没有任何其它的办法可以再重新获取到之前创建的子线程的实例。而且在一个Activity中创建的子线程，另一个Activity无法对其进行操作。但是Service就不同了，所有的Activity都可以与Service进行关联，然后可以很方便地操作其中的方法，即使Activity被销毁了，之后只要重新与Service建立关联，就又能够获取到原有的Service中Binder的实例。因此，使用Service来处理后台任务，Activity就可以放心地finish，完全不需要担心无法对后台任务进行控制的情况。

## Service 与 Thread 的区别

很多时候，你可能会问，为什么要用 Service，而不用 Thread 呢，因为用 Thread 是很方便的，比起 Service 也方便多了，下面我详细的来解释一下。

1). Thread：Thread 是程序执行的最小单元，它是分配CPU的基本单位。可以用 Thread 来执行一些异步的操作。

2). Service：Service 是android的一种机制，当它运行的时候如果是Local Service，那么对应的 Service 是运行在主进程的 main 线程上的。如：onCreate，onStart 这些函数在被系统调用的时候都是在主进程的 main 线程上运行的。如果是Remote Service，那么对应的 Service 则是运行在独立进程的 main 线程上。因此请不要把 Service 理解成线程，它跟线程半毛钱的关系都没有！

既然这样，那么我们为什么要用 Service 呢？其实这跟 android 的系统机制有关，我们先拿 Thread 来说。Thread 的运行是独立于 Activity 的，也就是说当一个 Activity 被 finish 之后，如果你没有主动停止 Thread 或者 Thread 里的 run 方法没有执行完毕的话，Thread 也会一直执行。因此这里会出现一个问题：当 Activity 被 finish 之后，你不再持有该 Thread 的引用。另一方面，你没有办法在不同的 Activity 中对同一 Thread 进行控制。

举个例子：如果你的 Thread 需要不停地隔一段时间就要连接服务器做某种同步的话，该 Thread 需要在 Activity 没有start的时候也在运行。这个时候当你 start 一个 Activity 就没有办法在该 Activity 里面控制之前创建的 Thread。因此你便需要创建并启动一个 Service ，在 Service 里面创建、运行并控制该 Thread，这样便解决了该问题（因为任何 Activity 都可以控制同一 Service，而系统也只会创建一个对应 Service 的实例）。

因此你可以把 Service 想象成一种消息服务，而你可以在任何有 Context 的地方调用 Context.startService、Context.stopService、Context.bindService，Context.unbindService，来控制它，你也可以在 Service 里注册 BroadcastReceiver，在其他地方通过发送 broadcast 来控制它，当然这些都是 Thread 做不到的。

## 如何保证 Service 在后台不被 kill

- onStartCommand方法，返回START_STICKY

START_STICKY 在运行onStartCommand后service进程被kill后，那将保留在开始状态，但是不保留那些传入的intent。不久后service就会再次尝试重新创建，因为保留在开始状态，在创建     service后将保证调用onstartCommand。如果没有传递任何开始命令给service，那将获取到null的intent。

START_NOT_STICKY 在运行onStartCommand后service进程被kill后，并且没有新的intent传递给它。Service将移出开始状态，并且直到新的明显的方法（startService）调用才重新创建。因为如果没有传递任何未决定的intent那么service是不会启动，也就是期间onstartCommand不会接收到任何null的intent。

START_REDELIVER_INTENT 在运行onStartCommand后service进程被kill后，系统将会再次启动service，并传入最后一个intent给onstartCommand。直到调用stopSelf(int)才停止传递intent。如果在被kill后还有未处理好的intent，那被kill后服务还是会自动启动。因此onstartCommand不会接收到任何null的intent。

- 提升service优先级

在AndroidManifest.xml文件中对于intent-filter可以通过android:priority = "1000"这个属性设置最高优先级，1000是最高值，如果数字越小则优先级越低，同时适用于广播。

- 提升service进程优先级

Android中的进程是托管的，当系统进程空间紧张的时候，会依照优先级自动进行进程的回收。Android将进程分为6个等级,它们按优先级顺序由高到低依次是:

1.前台进程( FOREGROUND_APP)
2.可视进程(VISIBLE_APP )
3.次要服务进程(SECONDARY_SERVER )
4.后台进程 (HIDDEN_APP)
5.内容供应节点(CONTENT_PROVIDER)
6.空进程(EMPTY_APP)

当service运行在低内存的环境时，将会kill掉一些存在的进程。因此进程的优先级将会很重要，可以使用startForeground 将service放到前台状态。这样在低内存时被kill的几率会低一些。

- onDestroy方法里重启service

service +broadcast  方式，就是当service走ondestory的时候，发送一个自定义的广播，当收到广播的时候，重新启动service；

- Application加上Persistent属性
- 监听系统广播判断Service状态

通过系统的一些广播，比如：手机重启、界面唤醒、应用状态改变等等监听并捕获到，然后判断我们的Service是否还存活，别忘记加权限啊。

## 什么是IntentService？有何优点？

IntentService也是一个Service，是Service的子类；IntentService和Service有所不同，通过Looper和Thread来解决标准Service中处理逻辑的阻塞的问题。

优点：Activity的进程，当处理Intent的时候，会产生一个对应的Service,Android的进程处理器现在会尽可能的不kill掉你。

**IntentService的使用场景与特点。**

> IntentService是Service的子类，是一个异步的，会自动停止的服务，很好解决了传统的Service中处理完耗时操作忘记停止并销毁Service的问题

优点：

- 一方面不需要自己去new Thread
- 另一方面不需要考虑在什么时候关闭该Service

onStartCommand中回调了onStart，onStart中通过mServiceHandler发送消息到该handler的handleMessage中去。最后handleMessage中回调onHandleIntent(intent)。

## IntentService 作用是什么

IntentService 是继承于 Service 并处理异步请求的一个类，在IntentService 内有一个工作线程来处理耗时操作，启动 IntentService 的方式和启动传统 Service 一样，同时，当任务执行完后，IntentService 会自动停止，而不需要我们去手动控制。另外，可以启动 IntentService 多次，而每一个耗时操作会以工作队列的方式在 IntentService 的 onHandleIntent 回调方法中执行，并且，每次只会执行一个工作线程，执行完第一个再执行第二个，以此类推。

## IntentService 与 Service的异同

- 直接创建一个默认的工作线程,该线程执行所有的intent传递给onStartCommand()区别于应用程序的主线程。
- 直接创建一个工作队列,将一个意图传递给你onHandleIntent()的实现,所以我们就永远不必担心多线程。
- 当请求完成后自己会调用stopSelf()，所以你就不用调用该方法了。
- 提供的默认实现onBind()返回null，所以也不需要重写这个方法。so easy啊
- 提供了一个默认实现onStartCommand(),将意图工作队列,然后发送到你onHandleIntent()实现。真是太方便了

# ContentProvider

参考：[http://blog.csdn.net/coder_pig/article/details/47858489](http://blog.csdn.net/coder_pig/article/details/47858489)

## ContentProvider 简介

ContentProvider(内容提供者)：为存储和获取数据提供统一的接口。可以在不同的应用程序之间共享数据。Android已经为常见的一些数据提供了默认的ContentProvider

1. ContentProvider 为存储和读取数据提供了统一的接口
2. 使用ContentProvider，应用程序可以实现 app 间数据共享
3. Android内置的许多数据都是使用ContentProvider形式，供开发者调用的(如视频，音频，图片，通讯录等)

## 请介绍下ContentProvider是如何实现数据共享的

创建一个属于你自己的Content provider或者将你的数据添加到一个已经存在的Content provider中，前提是有相同数据类型并且有写入Content provider的权限。

## ContentProvider使用方法

参考：[http://blog.csdn.net/juetion/article/details/17481039](http://blog.csdn.net/juetion/article/details/17481039)

# Broadcast

## 注册广播有哪几种方式,有什么区别

- 在应用程序的代码中注册

```java
registerReceiver（receiver，filter）；// 注册BroadcastReceiver
unregisterReceiver（receiver）；// 取消注册BroadcastReceiver
```

当BroadcastReceiver更新UI，通常会使用这样的方法注册。启动Activity时候注册BroadcastReceiver，Activity不可见时候，取消注册。

- 在androidmanifest.xml当中注册

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

## 一个 app 被杀掉进程后，是否还能收到广播

静态注册的常驻型广播接收器还能接收广播。

## Android引入广播机制的用意？

## 无序广播、有序广播

## 本地广播和全局广播有什么差别

全局广播 BroadcastReceiver 是针对应用间、应用与系统间、应用内部进行通信的一种方式。

本地广播 LocalBroadcastReceiver 仅在自己的应用内发送接收广播，也就是只有自己的应用能收到。因广播数据在本应用范围内传播，不用担心隐私数据泄露的问题。 不用担心别的应用伪造广播，造成安全隐患。 相比在系统内发送全局广播，它更高效。

# Intent

## Intent的使用方法，可以传递哪些数据类型。

通过查询Intent/Bundle的API文档，我们可以获知，Intent/Bundle支持传递基本类型的数据和基本类型的数组数据，以及String/CharSequence类型的数据和String/CharSequence类型的数组数据。而对于其它类型的数据貌似无能为力，其实不然，我们可以在Intent/Bundle的API中看到Intent/Bundle还可以传递Parcelable（包裹化，邮包）和Serializable（序列化）类型的数据，以及它们的数组/列表数据。

所以要让非基本类型和非String/CharSequence类型的数据通过Intent/Bundle来进行传输，我们就需要在数据类型中实现Parcelable接口或是Serializable接口。

参考：[http://blog.csdn.net/kkk0526/article/details/7214247](http://blog.csdn.net/kkk0526/article/details/7214247)

## 介绍一下Intent、IntentFilter

# Context

## Context区别

- Activity和Service以及Application的Context是不一样的,Activity继承自ContextThemeWraper.其他的继承自ContextWrapper
- 每一个Activity和Service以及Application的Context都是一个新的ContextImpl对象
- getApplication()用来获取Application实例的，但是这个方法只有在Activity和Service中才能调用的到。那么也许在绝大多数情况下我们都是在Activity或者Service中使用Application的，但是如果在一些其它的场景，比如BroadcastReceiver中也想获得Application的实例，这时就可以借助getApplicationContext()方法，getApplicationContext()比getApplication()方法的作用域会更广一些，任何一个Context的实例，只要调用getApplicationContext()方法都可以拿到我们的Application对象。
- Activity在创建的时候会new一个ContextImpl对象并在attach方法中关联它，Application和Service也差不多。ContextWrapper的方法内部都是转调ContextImpl的方法
- 创建对话框传入Application的Context是不可以的
- 尽管Application、Activity、Service都有自己的ContextImpl，并且每个ContextImpl都有自己的mResources成员，但是由于它们的mResources成员都来自于唯一的ResourcesManager实例，所以它们看似不同的mResources其实都指向的是同一块内存
- Context的数量等于Activity的个数 + Service的个数 + 1，这个1为Application

## Application Context 和 Activity Context 异同

## Context 的理解

#  Handler

## Handler 原理

Andriod提供了Handler 和 Looper 来满足线程间的通信。Handler先进先出原则。Looper类用来管理特定线程内对象之间的消息交换(MessageExchange)。

Looper: 一个线程可以产生一个Looper对象，由它来管理此线程里的MessageQueue(消息队列)。

Handler: 你可以构造Handler对象来与Looper沟通，以便push新消息到MessageQueue里;或者接收Looper从Message Queue取出)所送来的消息。

Message Queue(消息队列):用来存放线程放入的消息。

线程：UIthread 通常就是main thread，而Android启动程序时会替它建立一个MessageQueue。

参考：

[Handler、Looper、Message、MessageQueue基础流程分析](https://github.com/GeniusVJR/LearningNotes/blob/master/Part1/Android/%E7%BA%BF%E7%A8%8B%E9%80%9A%E4%BF%A1%E5%9F%BA%E7%A1%80%E6%B5%81%E7%A8%8B%E5%88%86%E6%9E%90.md)

## Handler消息机制，postDelayed会造成线程阻塞吗？对内存有什么影响？

## Handler, Looper的理解

## Handler,Message,Looper异步实现机制与源码分析

参考：

[Android 消息处理机制（Looper、Handler、MessageQueue,Message）](http://www.jianshu.com/p/02962454adf7)

## Handler有何作用？如何使用之（具体讲需要实现什么function）？

Android设计了Handler机制，由Handler来负责与子线程进行通讯，从而让子线程与主线程之间建立起协作的桥梁，使Android的UI更新的问题得到完美的解决。

参考：

[Handler有何作用？如何使用？](http://www.androidchina.net/313.html)

## Handler、Thread 和 HandlerThread 的区别

Handler 会关联一个单独的线程和消息队列。Handler默认关联主线程(即 UI 线程)，虽然要提供Runnable参数 ，但默认是直接调用Runnable中的run()方法。也就是默认下会在主线程执行，如果在这里面的操作会有阻塞，界面也会卡住。如果要在其他线程执行，可以使用 HandlerThread。

HandlerThread继承于Thread，所以它本质就是个Thread。与普通Thread的差别就在于，主要的作用是建立了一个线程，并且创立了消息队列，有自己的looper,可以让我们在自己的线程中分发和处理消息。

android.os.Handler可以通过Looper对象实例化，并运行于另外的线程中，Android提供了让Handler运行于其它线程的线程实现，也是就HandlerThread。HandlerThread对象start后可以获得其Looper对象，并且使用这个Looper对象实例Handler。

参考：

[Android中的Thread & HandlerThread & Handler](https://www.juwends.com/tech/android/android_thread_handler.html)

## 使用 Handler 时怎么避免引起内存泄漏

一旦Handler被声明为内部类，那么可能导致它的外部类不能够被垃圾回收。如果Handler是在其他线程（我们通常成为worker thread）使用Looper或MessageQueue（消息队列），而不是main线程（UI线程），那么就没有这个问题。如果Handler使用Looper或MessageQueue在主线程（main thread），你需要对Handler的声明做如下修改： 

1. 声明Handler为static类；
2. 在外部类中实例化一个外部类的WeakReference（弱引用）并且在Handler初始化时传入这个对象给你的Handler；
3. 将所有引用的外部类成员使用WeakReference对象。

```java
private static class MyHandler extends Handler {          
  private final WeakReference<HandlerActivity2> mActivity; 
  
  public MyHandler(HandlerActivity2 activity) {              
  	mActivity = new WeakReference<HandlerActivity2>(activity);          
  }
  
  @Override          
  public void handleMessage(Message msg) {              
  	System.out.println(msg);             
    if (mActivity.get() == null) {                  
    	return;              
    }              
    mActivity.get().todo();          
  }      
}  
```

当Activity finish后 handler对象还是在Message中排队。 还是会处理消息，这些处理有必要？正常Activitiy finish后，已经没有必要对消息处理，那需要怎么做呢？解决方案也很简单，在Activity onStop或者onDestroy的时候，取消掉该Handler对象的Message和Runnable。通过查看Handler的API，它有几个方法：removeCallbacks(Runnable r)和removeMessages(int what)等。

```java
/**    * 一切都是为了不要让mHandler拖泥带水    */   
@Override   
public void onDestroy() {       
  mHandler.removeMessages(MESSAGE_1);       
  mHandler.removeMessages(MESSAGE_2);       
  mHandler.removeMessages(MESSAGE_3);        
  // ... ...         
  mHandler.removeCallbacks(mRunnable);         
  // ... ...   
}  
```

参考：

[Handler内存泄漏分析及解决](http://www.jianshu.com/p/cb9b4b71a820)

# AsyncTask

## AsyncTask原理

参考：

[深入理解AsyncTask的工作原理](http://www.cnblogs.com/absfree/p/5357678.html)

## 网络请求是怎么做的异步呢？什么情况下用Handler，什么情况下用AsyncTask

## Handler和AsyncTask的区别
这俩类都是用来实现异步的，其中AsyncTask的集成度较高，使用简单，Handler则需要手动写Runnable或者Thread的代码；另外，由于AsyncTask内部实现了一个非常简单的线程池，实际上是只适用于轻量级的异步操作的，一般不应该用于网络操作。（感谢网友指正，AsyncTask 通过重写的方式是可以用于长耗时操作的，而我只考虑了直接使用的情况就说它不适合网络操作，是不对的。）我问他Handler和AsyncTask的区别，一方面是因为他说用AsyncTask联网，因此我认为他对AsyncTask并不熟悉；但更重要的是在我问他实现异步的具体手段的时候，他同时提到了Handler和AsyncTask——用这种“混搭”的使用方式来写联网框架，就算不考虑AsyncTask的可用性，也显得非常怪异，这听起来更像是在“列举Android实现异步操作最常用的类”，而非“讲述实现网络异步操作的具体方式”。也就是说，我听了这句话后开始怀疑他封装过联网框架这件事的真实性。但我只是怀疑，并不确定，因此接着问了我想问的。

参考：

https://www.zhihu.com/question/30804052

## AsyncTask的优缺点？能否同时并发100+AsyncTask呢？

参考：

[AsyncTask到底是什么](http://www.jianshu.com/p/3a4d25efce46)

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

|                                | **Service**                              | **Thread**                               | **IntentService**                        | **AsyncTask**                            |
| ------------------------------ | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| **When to use ?**              | Task with no UI, but shouldn't be too long. Use threads within service   for long tasks. | - Long task in general.- For tasks in parallel use Multiple threads (traditional mechanisms) | - Long task **usually** with no communication to main thread.**(Update)**- If communication is required, can use main thread handler or   broadcast intents- When callbacks are needed (Intent triggered tasks). | - Small task having to communicate with main thread.- For tasks in parallel use multiple instances OR Executor |
| **Trigger**                    | Call to methodonStartService()           | Thread start() method                    | Intent                                   | Call to method execute()                 |
| **Triggered From (thread)**    | Any thread                               | Any Thread                               | Main Thread (Intent is received on main thread and then worker thread   is spawed) | Main Thread                              |
| **Runs On (thread)**           | Main Thread                              | Its own thread                           | Separate worker thread                   | Worker thread. However, Main thread methods may be invoked in between   to publish progress. |
| **Limitations /****Drawbacks** | May block main thread                    | - Manual thread management- Code may become difficult to read | - Cannot run tasks in parallel.- Multiple intents are queued on the same worker thread. | - one instance can only be executed once (hence cannot run in a loop) - Must be created and executed from the Main thread |

## 线程同步

- 使用 synchronized 关键字创建 synchronized 方法。
- 使用 synchronized 创建同步代码块。

参考：

http://www.itzhai.com/java-based-notebook-thread-synchronization-problem-solving-synchronization-problems-synchronized-block-synchronized-methods.html#2.1%E3%80%81%E4%BD%BF%E7%94%A8synchronized%E5%85%B3%E9%94%AE%E5%AD%97%E5%88%9B%E5%BB%BAsynchronized%E6%96%B9%E6%B3%95%EF%BC%9A

http://www.juwends.com/tech/android/android-inter-thread-comm.html

# 进程

## 进程优先级

1. 前台进程：即与用户正在交互的Activity或者Activity用到的Service等，如果系统内存不足时前台进程是最后被杀死的
2. 可见进程：可以是处于暂停状态(onPause)的Activity或者绑定在其上的Service，即被用户可见，但由于失去了焦点而不能与用户交互
3. 服务进程：其中运行着使用startService方法启动的Service，虽然不被用户可见，但是却是用户关心的，例如用户正在非音乐界面听的音乐或者正在非下载页面自己下载的文件等；当系统要空间运行前两者进程时才会被终止
4. 后台进程：其中运行着执行onStop方法而停止的程序，但是却不是用户当前关心的，例如后台挂着的QQ，这样的进程系统一旦没了有内存就首先被杀死
5. 空进程：不包含任何应用程序的程序组件的进程，这样的进程系统是一般不会让他存在的

## AIDL 解决了什么问题

AIDL (Android Interface Definition Language) 是一种IDL 语言，用于生成可以在 Android 设备上两个进程之间进行进程间通信(interprocess communication, IPC)的代码。如果在一个进程中（例如Activity）要调用另一个进程中（例如 Service, 设置了属性 android:process=":remote" 后，Service 就会运行在另外一个进程）对象的操作，就可以使用AIDL生成可序列化的参数。 AIDL IPC机制是面向接口的，像COM或Corba一样，但是更加轻量级。它是使用代理类在客户端和实现端传递数据。

## AIDL的全称是什么？如何工作？能处理哪些类型的数据

ADIL是一种接口定义语言，用于约束两个进程之间的通信规则，供编译器生成代码，实现android设备之间的进程通信。

进程之间的通信信息首先会被转换成AIDL协议消息，然后发送给对方，对方受到AIDL协议消息后在转换成相应的对象。AIDL支持类型包括java基础类型和String，List，Map,CharSequence,如果使用自定类型，必须实现Parcelable接口

## Broadcast、Content Provider 和 AIDL的区别和联系

这3种都可以实现跨进程的通信，那么从效率，适用范围，安全性等方面来比较的话他们3者之间有什么区别？

Broadcast：用于发送和接收广播！实现信息的发送和接收！

AIDL：用于不同程序间服务的相互调用！实现了一个程序为另一个程序服务的功能！
Content Provider:用于将程序的数据库人为地暴露出来！实现一个程序可以对另个程序的数据库进行相对用的操作！

Broadcast,既然是广播，那么它的优点是：注册了这个广播接收器的应用都能够收到广播，范围广。缺点是：速度慢点，而且必须在一定时间内把事情处理完(onReceive执行必须在几秒之内)，否则的话系统给出ANR。

AIDL，是进程间通信用的，类似一种协议吧。优点是：速度快(系统底层直接是共享内存)，性能稳，效率高，一般进程间通信就用它。

Content Provider,因为只是把自己的数据库暴露出去，其他程序都可以来获取数据，数据本身不是实时的，不像前两者,只是起个数据供应作用。一般是某个成熟的应用来暴露自己的数据用的。 你要是为了进程间通信，还是别用这个了，这个又不是实时数据。

## 进程间传输方式

## Android进程间通信，Binder机制

## Android跨进程通讯的方式

## Android Mashup设计的理解

# 触摸事件

## View 的触摸事件分发机制

**基础知识**

1. 所有Touch事件都被封装成了MotionEvent对象，包括Touch的位置、时间、历史记录以及第几个手指(多指触摸)等。
2.  事件类型分为ACTION_DOWN, ACTION_UP, ACTION_MOVE, ACTION_POINTER_DOWN, ACTION_POINTER_UP, ACTION_CANCEL，每个事件都是以ACTION_DOWN开始ACTION_UP结束。
3. 对事件的处理包括三类，分别为传递——dispatchTouchEvent()函数、拦截——onInterceptTouchEvent()函数、消费——onTouchEvent()函数和OnTouchListener

**传递流程**

1.  事件从Activity.dispatchTouchEvent()开始传递，只要没有被停止或拦截，从最上层的View(ViewGroup)开始一直往下(子View)传递。子View可以通过onTouchEvent()对事件进行处理。
2. 事件由父View(ViewGroup)传递给子View，ViewGroup可以通过onInterceptTouchEvent()对事件做拦截，停止其往下传递。
3. 如果事件从上往下传递过程中一直没有被停止，且最底层子View没有消费事件，事件会反向往上传递，这时父View(ViewGroup)可以进行消费，如果还是没有被消费的话，最后会到Activity的onTouchEvent()函数。
4.  如果View没有对ACTION_DOWN进行消费，之后的其他事件不会传递过来。
5. OnTouchListener优先于onTouchEvent()对事件进行消费。

上面的消费即表示相应函数返回值为true。

- View不处理事件流程图：

![](http://o9sn2y8lr.bkt.clouddn.com/16-10-14/18162238.jpg)

- View处理事件流程图：`View.onTouchEvent()` 返回true

![](http://o9sn2y8lr.bkt.clouddn.com/16-10-14/48753378.jpg)

- 拦截事件流程图：`ViewGroup.onInterceptTouchEvent()` 对MOVE、UP事件进行拦截

![](http://o9sn2y8lr.bkt.clouddn.com/16-10-14/44984087.jpg)

参考：

[Android Touch事件传递机制](http://www.trinea.cn/android/touch-event-delivery-mechanism/)

[事件分发机制](http://www.jianshu.com/p/e99b5e8bd67b)

## onInterceptTouchEvent()和onTouchEvent()的区别？

onInterceptTouchEvent()用于拦截触摸事件。


onTouchEvent()用于处理触摸事件。

## view的事件冲突处理

# View

## View 绘制流程

当 Activity 接收到焦点的时候，它会被请求绘制布局，该请求由 Android framework 处理。绘制是从根节点开始，对布局树进行 measure 和 draw。整个 View 树的绘图流程在ViewRoot.java类的 performTraversals() 函数展开，该函数所做 的工作可简单概况为是否需要重新计算视图大小(measure)、是否需要重新安置视图的位置(layout)、以及是否需要重绘(draw)，流程图如下：

![](http://o9sn2y8lr.bkt.clouddn.com/view_mechanism_flow.png)

![](http://o9sn2y8lr.bkt.clouddn.com/view_draw_method_chain.png)

View的绘制流程是从ViewRoot的`performTraversals()`方法开始，依次经过`measure()`，`layout()`和`draw()`三个过程才最终将一个View绘制出来。

参考：

[公共技术点之 View 绘制流程](http://www.codekk.com/blogs/detail/54cfab086c4761e5001b253f)

## Android 绘图机制原理

参考：

[Android中的绘制机制](http://blog.csdn.net/leehong2005/article/details/7307456)

## requertlayout onlayout onDraw drawChild 的区别和联系

- requestLayout()方法 ：会导致调用measure()过程 和 layout()过程 。 将会根据标志位判断是否需要ondraw。
- onLayout()方法：如果该View是ViewGroup对象，需要实现该方法，对每个子视图进行布局。
- 调用onDraw()方法绘制视图本身，每个View都需要重载该方法，ViewGroup不需要实现该方法。
- drawChild()去重新回调每个子视图的draw()方法。


## View 刷新机制

在Android的布局体系中，父View负责刷新、布局显示子View；而当子View需要刷新时，则是通知父View来完成。

子View调用invalidate时，首先找到自己父View(View的成员变量mParent记录自己的父View)，然后将AttachInfo中保存的信息告诉父View刷新自己。

在invalidate中，调用父View的invalidateChild，这是一个从第向上回溯的过程，每一层的父View都将自己的显示区域与传入的刷新Rect做交集。

这个向上回溯的过程直到ViewRoot那里结束，由ViewRoot对这个最终的刷新区域做刷新。

参考：

[Android View刷新机制](http://blog.csdn.net/chenzhiqin20/article/details/8628952)

## invalidata() 和 postInvalidata() 的区别及使用

- invalidata() 必须在 UI 线程中调用，所以一般都是配合 Handler 使用。
- postInvalidata() 可以在其他线程直接调用。

## notifyDataSetChanged和notifyDataSetInvalidated的区别

- notifyDataSetInvalidated()，会重绘整个控件（还原到初始状态）
- notifyDataSetChanged()，重绘当前可见区域

## SurfaceView和View的区别是什么？

SurfaceView中采用了双缓存技术，在单独的线程中更新界面。而View在UI线程中更新界面。

## RemoteView在哪些功能中使用

在 AppWidget（桌面小插件）和 Notification（通知栏）中使用。

参考：

[Android widget 之RemoteView](http://www.cnblogs.com/playing/archive/2011/04/22/2024775.html)

# 自定义 View

## 自定义View相关方法

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

## 如何自定义控件

## 有哪些实现自定义控件的方法？

自定义控件的实现有三种方式，分别是：组合控件、自绘控件和继承控件。

参考：

[Android自定义View的三种实现方式](http://www.cnblogs.com/jiayongji/p/5560806.html)

## 优化自定义 View

为了加速你的view，对于频繁调用的方法，需要尽量减少不必要的代码。先从onDraw开始，需要特别注意不应该在这里做内存分配的事情，因为它会导致GC，从而导致卡顿。在初始化或者动画间隙期间做分配内存的动作。不要在动画正在执行的时候做内存分配的事情。

你还需要尽可能的减少onDraw被调用的次数，大多数时候导致onDraw都是因为调用了invalidate().因此请尽量减少调用invaildate()的次数。如果可能的话，尽量调用含有4个参数的invalidate()方法而不是没有参数的invalidate()。没有参数的invalidate会强制重绘整个view。

另外一个非常耗时的操作是请求layout。任何时候执行requestLayout()，会使得Android UI系统去遍历整个View的层级来计算出每一个view的大小。如果找到有冲突的值，它会需要重新计算好几次。另外需要尽量保持View的层级是扁平化的，这样对提高效率很有帮助。

如果你有一个复杂的UI，你应该考虑写一个自定义的ViewGroup来执行他的layout操作。与内置的view不同，自定义的view可以使得程序仅仅测量这一部分，这避免了遍历整个view的层级结构来计算大小。这个PieChart 例子展示了如何继承ViewGroup作为自定义view的一部分。PieChart 有子views，但是它从来不测量它们。而是根据他自身的layout法则，直接设置它们的大小。

## 自定义view控件以及自定义属性的使用

## NavigationDrawer，PageAdapter等UI模式的使用和定制

# Layout 布局

## Android中常用的五种布局

* FrameLayout（框架布局）
* LinearLayout （线性布局）
* AbsoluteLayout（绝对布局）
* RelativeLayout（相对布局）
* TableLayout（表格布局）

## LinearLayout和RelativeLayout性能对比

1. RelativeLayout会让子View调用2次onMeasure，LinearLayout 在有weight时，也会调用子View2次onMeasure
2. RelativeLayout的子View如果高度和RelativeLayout不同，则会引发效率问题，当子View很复杂时，这个问题会更加严重。如果可以，尽量使用padding代替margin。
3. 在不影响层级深度的情况下,使用LinearLayout和FrameLayout而不是RelativeLayout。

最后再思考一下文章开头那个矛盾的问题，为什么Google给开发者默认新建了个RelativeLayout，而自己却在DecorView中用了个LinearLayout。因为DecorView的层级深度是已知而且固定的，上面一个标题栏，下面一个内容栏。采用RelativeLayout并不会降低层级深度，所以此时在根节点上用LinearLayout是效率最高的。而之所以给开发者默认新建了个RelativeLayout是希望开发者能采用尽量少的View层级来表达布局以实现性能最优，因为复杂的View嵌套对性能的影响会更大一些。

参考 ：

http://www.jianshu.com/p/8a7d059da746

## Android中px,sp,dip,dp的区别与联系

px: pixel，即像素，1px代表屏幕上的一个物理的像素点。但px单位不被建议使用。因为同样像素大小的图片在不同手机显示的实际大小可能不同。要用到px的情况是需要画1像素表格线或阴影线的时候，如果用其他单位画则会显得模糊。

dip (dp): device independent pixel。dp (dip)是最常用也是最难理解的尺寸单位。与像素密度密切相关。Android系统定义了四种像素密度：低（120dpi）、中（160dpi）、高（240dpi）和超高（320dpi），它们对应的dp到px的系数分别为0.75、1、1.5和2，这个系数乘以dp长度就是像素数。例如界面上有一个长度为“80dp”的图片，那么它在240dpi的手机上实际显示为80x1.5=120px，在320dpi的手机上实际显示为80x2=160px。如果你拿这两部手机放在一起对比，会发现这个图片的物理尺寸“差不多”，这就是使用dp作为单位的效果。

sp: Scale-independent Pixel，即与缩放无关的抽象像素。sp和dp很类似但唯一的区别是，Android系统允许用户自定义文字尺寸大小（小、正常、大、超大等等），当文字尺寸是“正常”时，1sp=1dp=0.00625英寸，而当文字尺寸是“大”或“超大”时，1sp>1dp=0.00625英寸。类似我们在windows里调整字体尺寸以后的效果——窗口大小不变，只有文字大小改变。

## asset目录与res目录的区别。

res 目录下面有很多文件，例如 drawable,mipmap,raw 等。res 下面除了 raw 文件不会被压缩外，其余文件都会被压缩。同时 res目录下的文件可以通过R 文件访问。

Asset 也是用来存储资源，但是 asset 文件内容只能通过路径或者 AssetManager 读取。

参考：

 [官方文档](https://developer.android.com/studio/projects/index.html)

## Android屏幕适配

# 动画

## Android属性动画特性

Android 起初有两种动画：Frame Animation（逐帧动画） Tween Animation（补间动画）,但是在用的时候发现这两种动画有时候并不能满足我们的一些需要，所以Google在Androi3.0的时候推出了（Property Animation）属性动画,至于为什么前边的两种动画不能满足我们的需要，请往下看：

Frame Animation(逐帧动画)

逐帧动画就是UI设计多张图片组成一个动画，然后将它们组合链接起来进行动画播放。该方式类似于早期电影的制作原理：具体实现方式就不多说了，你只需要让你们的UI出多张图片，然后你顺序的组合就可以（前提是UI给您做图）

Tween Animation(补间动画) 

 Tween Animation：是对某个View进行一系列的动画的操作，包括淡入淡出（Alpha），缩放（Scale），平移（Translate），旋转（Rotate）四种模式

Tween Animation(补间动画)的一些缺点：

1. Tween Animation（补间动画）只是针对于View，超脱了View就无法操作了，这句话的意思是：假如我们需要对一个Button，ImageView，LinearLayout或者是其他的继承自View的各种组件进行动画的操作时，Tween Animation是可以帮我们完成我们需要完成的功能的，但是如果我们需要用到对一个非View的对象进行动画操作的话，那么补间动画就没办法实现了。举个例子：比如我们有一个自定义的View，在这个View中有一个Point对象用于管理坐标，然后在onDraw()方法中的坐标就是根据该Pointde坐标值进行绘制的。也就是说，如果我们可以对Point对象进行动画操作，那么整个自定义的View，那么整个自继承View的当前类就都有了动画，但是我们的目的是不想让View有动画，只是对动画中的Point坐标产生动画，这样补间动画就不能满足了。
2. Tween Animation动画有四种动画操作（移动，缩放，旋转，淡入淡出），但是我们现在有个需求就是将当前View的背景色进行改变呢？抱歉Tween Animation是不能帮助我们实现的。
3. Tween Animation动画只是改变View的显示效果而已，但是不会真正的去改变View的属性，举个例子：我们现在屏幕的顶部有一个小球，然后通过补间动画让他移动到右下角，然后我们给这个小球添加了点击事件，希望位置移动到右下角的时候点击小球能的放大小球。但是点击事件是绝对不会触发的，原因是补间动画只是将该小球绘制到了屏幕的右下角，实际这个小球还是停在屏幕的顶部，所以你在右下角点击是没有任何反应的。

Property Animatior(属性动画)

属性动画是Android3.0之后引进的，它更改的是动画的实际属性，在Tween Animation(补间动画)中，其改变的是View的绘制效果，真正的View的属性是改变不了的，比如你将你的Button位置移动之后你再次点击Button是没有任何点击效果的，或者是你如何缩放你的Button大小，缩放后的有效的点击区域还是只有你当初初始的Button的大小的点击区域，其位置和大小的属性并没有改变。而在Property Animator(属性动画)中，改变的是动画的实际属性，如Button的缩放，Button的位置和大小属性值都会发生改变。而且Property Animation不止可以应用于View，还可以应用于任何对象，Property Animation只是表示一个值在一段时间内的改变，当值改变时要做什么事情完全是你自己决定的。

## Android动画框架实现原理

Animation 框架定义了透明度，旋转，缩放和位移几种常见的动画，而且控制的是整个View。实现原理：

每次绘制视图时，View 所在的 ViewGroup 中的 drawChild 函数获取该View 的 Animation 的 Transformation 值，然后调用canvas.concat(transformToApply.getMatrix())，通过矩阵运算完成动画帧，如果动画没有完成，继续调用 invalidate() 函数，启动下次绘制来驱动动画，动画过程中的帧之间间隙时间是绘制函数所消耗的时间，可能会导致动画消耗比较多的CPU资源，最重要的是，动画改变的只是显示，并不能相应事件。

参考：

[Android动画原理分析](http://www.cnblogs.com/kross/p/4087780.html)

# 图片缓存

## 图片三级缓存实现？自己设计一个图片加载框架

参考：

[Android中图片的三级缓存](http://www.jianshu.com/p/2cd59a79ed4a)

[开源选型之Android三大图片缓存原理、特性对比](http://www.csdn.net/article/2015-10-21/2825984) 

## 对 LruCache 的理解

参考：

[内存缓存LruCache实现原理](http://www.cnblogs.com/liuling/p/2015-9-24-1.html)

[Android提供的LruCache类简介](http://blog.csdn.net/linghu_java/article/details/8574102)

## DiskLruCache

参考：

[Android DiskLruCache完全解析，硬盘缓存的最佳方案](http://blog.csdn.net/sinyu890807/article/details/28863651)

## 缓存算法

## Bitmap的分析与使用

参考：

[Android 从具体实例分析Bitmap使用时候内存注意点](http://blog.csdn.net/wuyuxing24/article/details/51675133)

[Android之Bitmap大图加载处理](http://blog.csdn.net/xu_fu/article/details/8262153)

[Loading Large Bitmaps Efficiently](https://developer.android.com/training/displaying-bitmaps/load-bitmap.html)

## Bitmap 压缩

```java
public static Bitmap create(byte[] bytes, int maxWidth, int maxHeight) {
		//上面的省略了
        option.inJustDecodeBounds = true;
        BitmapFactory.decodeByteArray(bytes, 0, bytes.length, option);
        int actualWidth = option.outWidth;
        int actualHeight = option.outHeight;

        // 计算出图片应该显示的宽高
        int desiredWidth = getResizedDimension(maxWidth, maxHeight, actualWidth, actualHeight);
        int desiredHeight = getResizedDimension(maxHeight, maxWidth, actualHeight, actualWidth);

        option.inJustDecodeBounds = false;
        option.inSampleSize = findBestSampleSize(actualWidth, actualHeight,
                desiredWidth, desiredHeight);
        Bitmap tempBitmap = BitmapFactory.decodeByteArray(bytes, 0, bytes.length, option);

        // 做缩放
        if (tempBitmap != null
                && (tempBitmap.getWidth() > desiredWidth || tempBitmap
                .getHeight() > desiredHeight)) {
            bitmap = Bitmap.createScaledBitmap(tempBitmap, desiredWidth,
                    desiredHeight, true);
            tempBitmap.recycle();
        } else {
            bitmap = tempBitmap;
        }
    }

    return bitmap;
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

## ARGB_8888格式图片占用内存大小

1 byte = 8 bit

8+8+8+8 = 32    32/8 = 4 byte 一个像素就占4byte

参考：

[你的 Bitmap 究竟占多大内存](http://bugly.qq.com/bbs/forum.php?mod=viewthread&tid=498) 

## 如何判断本地缓存的时候数据需要从网络端获取

## 图片加载机制

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

参考：

[Android ListView工作原理完全解析，带你从源码的角度彻底理解](http://blog.csdn.net/sinyu890807/article/details/44996879)

## ViewHolder

参考：

[ListView中convertView和ViewHolder的工作原理](http://blog.csdn.net/bill_ming/article/details/8817172)

## ListView 下拉刷新、上拉加载更多实现原理

参考：

[下拉刷新及滚动到底部加载更多的Listview使用](http://www.trinea.cn/android/dropdown-to-refresh-and-bottom-load-more-listview/) — 优先使用该开源实现

[Android ListView下拉/上拉刷新：设计原理与实现](http://blog.csdn.net/zhangphil/article/details/47036177)

[一个支持下拉上拉的开源库](https://github.com/Aspsine/IRecyclerView)

## RecyclerView和ListView的异同

- RecyclerView 自带 ViewHolder；而 ListView 则需要自定义。
- RecyclerView 支持水平和垂直滚动；而 ListView 只支持垂直滚动。
- RecyclerView 提供默认的列表项动画实现，例如：添加、删除和移动列表项动画。
- ListView通过AdapterView.OnItemClickListener接口来监听点击事件。而RecyclerView则通过RecyclerView.OnItemTouchListener接口来监听触摸事件。它虽然增加了实现的难度，但是却给予开发人员拦截触摸事件更多的控制权限。
- ListView可以设置选择模式，并添加MultiChoiceModeListener；而 RecyclerView 没有该功能。

参考：

http://www.tuicool.com/articles/aeeaQ3J

http://blog.csdn.net/sanjay_f/article/details/48830311

## 能否讲讲你用过的adapter？

# 容器

## SparseArray 和 HashMap 的区别

| 类           | cpu                                      | 内存                              | 适用场景                                     |
| ----------- | ---------------------------------------- | ------------------------------- | ---------------------------------------- |
| HashMap     | 增、删、查找速度较快                               | 双倍扩容、不做空间整理，内存使用效率低             | 数据量较大或内存空间相对宽裕                           |
| ArrayMap    | 增、删、查速度较慢                                | size大于8扩容时，只增大当前数组大小的一半，做空间收缩整理 | 数据量小于1000时，速度相对差别不大，可替代HashMap           |
| SparseArray | 增、查速度较慢，由于延迟删除机制，删速度比ArrayMap快，比HashMap慢 | 矩阵压缩，大大减少了存储空间，节约内存             | 避免了key的自动装箱，空间压缩等机制，使得其在key是Integer、Long，且数据量较小场景下性能最优 |

参考：

[HashMap、ArrayMap、SparseArray分析比较](http://blog.csdn.net/chen_lifeng/article/details/52057427)

# 内存相关

## 什么情况会导致内存泄漏

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

参考：

[Android内存泄漏总结](https://github.com/GeniusVJR/LearningNotes/blob/master/Part1/Android/Android%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F%E6%80%BB%E7%BB%93.md)

[Handler内存泄漏分析及解决](http://www.jianshu.com/p/cb9b4b71a820)

## 什么情况会导致 OOM

1. 加载对象过大，例如：图片、文件等。
2. 相应资源过多，没有来不及释放。
3. 内存泄漏。

## 如何避免 OOM：

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


参考：

[Android内存优化之OOM](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0920/3478.html)

## Android 为每个应用程序分配的内存大小是多少

Android 程序内存一般限制在16M，也有的是24M

## 查看每个应用程序最高可用内存

```java
int maxMemory = (int) (Runtime.getRuntime().maxMemory() / 1024);  
Log.d("TAG", "Max memory is " + maxMemory + "KB");  
```

```java
ActivityManager manager = (ActivityManager)getSystemService(Context.ACTIVITY_SERVICE);  
//可用堆内存，单个应用可以使用的最大内存，如果应用内存使用超过这个值，就报OOM  
int heapgrowthlimit = manager.getMemoryClass();  
//进程内存空间分配的最大值，表示的是单个虚拟机可用的最大内存  
int heapsize = manager.getLargeMemoryClass();  
L.d("heapgrowthlimit = "+heapgrowthlimit+"m"+", heapsize = "+heapsize+"");  
```

```java
//应用程序最大可用内存  
int maxMemory = ((int) Runtime.getRuntime().maxMemory())/1024/1024;  
//应用程序已获得内存  
long totalMemory = ((int) Runtime.getRuntime().totalMemory())/1024/1024;  
//应用程序已获得内存中未使用内存  
long freeMemory = ((int) Runtime.getRuntime().freeMemory())/1024/1024;  
System.out.println("---> maxMemory="+maxMemory+"M,totalMemory="+totalMemory+"M,freeMemory="+freeMemory+"M"); 
```

## Android中弱引用与软引用的应用场景。

## 预防内存泄漏！擅用WeakReference!

所有从类外部传来的对象（特别对于Context,View,Fragmet,Activity对象），如果要将其放进类内部的容器对象或者静态类中引用，请一直用WeakReference包装！比如在TabLayout的源码中，容器对象或者静态类中引用，用WeakReference包装。在TabLayoutOnPageChangeListener中，就为TabLayout做了WeakReference wrap。

# ANR

## ANR 定位和修正

如果开发机器上出现问题，我们可以通过查看/data/anr/traces.txt即可，最新的ANR信息在最开始部分。

出现 ANR 原因：

- 应用在5秒内未响应用户的输入事件（如按键或者触摸）。
- BroadcastReceiver在10秒内未完成相关的处理。
- Service在 20秒内无法处理完成。

- 主线程被IO操作（从4.0之后网络IO不允许在主线程中）阻塞。
- 主线程中存在耗时的计算。
- 主线程中错误的操作，比如Thread.wait或者Thread.sleep等。

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
  4. 使用 AsyncTask 时：在doInBackground()方法中执行耗时操作；在onPostExecuted()更新UI 。
  5. 使用Handler实现异步任务时：在子线程中处理耗时操作，处理完成之后，通过handler.sendMessage()传递处理结果；在handler的handleMessage()方法中更新 UI 或者使用handler.post()方法将消息放到Looper中。

## 什么是ANR，如何避免ANR。

## 什么是FC？如何避免FC的发生，另外FC发生时如何捕获相应的uncaught exception？

# 性能优化

## 怎么对 Android APP 进行性能优化

**合理管理内存**

1. 节制的使用Service
2. 当界面不可见时释放内存
3. 当内存紧张时释放内存
4. 避免在Bitmap上浪费内存
5. 使用优化过的数据集合
6. 知晓内存的开支情况
7. 谨慎使用抽象编程
8. 尽量避免使用依赖注入框架
9. 使用多个进程
10. 避免内存泄漏

**高性能编码优化**

1. 避免创建不必要的对象
2. 静态优于抽象
3. 对常量使用static final修饰符
4. 使用增强型for循环语法
5. 多使用系统封装好的API
6. 避免在内部调用Getters/Setters方法

**布局优化技巧**

1. 重用布局文件
2. 仅在需要时才加载布局

参考：

[Android 性能优化](https://github.com/GeniusVJR/LearningNotes/blob/master/Part1/Android/Android%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96.md)

[性能优化系列总篇](http://www.trinea.cn/android/performance/)

## Android APP 内存分析工具有哪些

Square 开源库 LeakCanary

参考：

[利用 LeakCanary 来检查 Android 内存泄漏](http://www.jianshu.com/p/0049e9b344b0)

## 屏幕适配经验

参考：

[Android屏幕适配全攻略](http://www.cocoachina.com/android/20151030/13971.html)

# 数据存储

## 数据存储，数据持久化的方式有哪些

- 网络
- SharedPreference： 除SQLite数据库外，另一种常用的数据存储方式，其本质就是一个xml文件，常用于存储较简单的参数设置。
- SQLite：SQLite是一个轻量级的数据库，支持基本的SQL语法，是常被采用的一种数据存储方式。Android为此数据库提供了一个名为SQLiteDatabase的类，封装了一些操作数据库的api
- File： 即常说的文件（I/O）存储方法，常用语存储大数量的数据，但是缺点是更新数据将是一件困难的事情。
- ContentProvider: Android系统中能实现所有应用程序共享的一种数据存储方式，由于数据通常在各应用间的是互相私密的，所以此存储方式较少使用，但是其又是必不可少的一种存储方式。例如音频，视频，图片和通讯录，一般都可以采用此种方式进行存储。每个Content Provider都会对外提供一个公共的URI（包装成Uri对象），如果应用程序有数据需要共享时，就需要使用Content Provider为这些数据定义一个URI，然后其他的应用程序就通过Content Provider传入这个URI来对数据进行操作。

## 文件和数据库哪个效率高

1. 数据量非常少，可以使用文件。
2. 如果涉及到查询等操作，并且数据量大，则应该使用数据库。

## SharedPreference 实现

## 如何导入外部数据库

把数据库存放到 res/raw 目录下，然后在第一次安装启动应用的时间把该数据库拷贝到应用的内部存储空间即 android 系统下的 /data/data/packagename/ 目录下。

## 谈谈 SQLite

## SQLite 数据库升级更新如何保留原来数据

在使用数据库之前，基本上会自定义一个类继承自SQLiteOpenHelper。该类的其中一个构造函数形式是这样的（另一个多出来一个DatabaseErrorHandler）：   

```java
public SQLiteOpenHelper(Context context, String name, 
  CursorFactory factory, int version) {
  this(context, name, factory, version, null);
}
```

这个构造函数里面的version参数即是我们设定的版本号。第一次使用数据库时传递的这个版本将被系统记录，并调用SQLiteOpenHelper#onCreate()方法进行建表操作。后续传入的版本如果比这个高，则会调用SQLiteOpenHelper#onUpgrade()方法进行升级。 

**跨越版本的升级**  

处理好了单个版本的升级，还有一个更加棘手的问题：如果应用程序发布了多个版本，以致出现了三个以上数据库版本， 如何确保所有的用户升级应用后数据库都能用呢？有两种方式：  

1. 确定 **相邻版本** 的差别，从版本1开始依次迭代更新，先执行v1到v2，再v2到v3……   
2. 为 **每个版本** 确定与现在数据库的差别，为每个case撰写专门的升级代码。   

方式一的优点是每次更新数据库的时候只需要在onUpgrade方法的末尾加一段从上个版本升级到新版本的代码，易于理解和维护，缺点是当版本变多之后，多次迭代升级可能需要花费不少时间，增加用户等待；  

方式二的优点则是可以保证每个版本的用户都可以在消耗最少的时间升级到最新的数据库而无需做无用的数据多次转存，缺点是强迫开发者记忆所有版本数据库的完整结构，且每次升级时onUpgrade方法都必须全部重写。  

以上简单分析了两种方案的优缺点，它们可以说在花费时间上是刚好相反的，至于如何取舍，可能还需要结合具体情况分析。  

参考：

[Android 数据库升级完整解决方案](http://www.tuicool.com/articles/2iyU3aQ)

## SQLite 性能优化

参考：

[性能优化第一篇——数据库性能优化](http://www.trinea.cn/android/database-performance/)

# 网络通讯

## 描述一次网络请求的流程

1. 建立TCP连接。在HTTP工作开始之前，Web浏览器首先要通过网络与Web服务器建立连接，该连接是通过TCP来完成的，该协议与IP协议共同构建Internet，即著名的TCP/IP协议族，因此Internet又被称作是TCP/IP网络。HTTP是比TCP更高层次的应用层协议，根据规则，只有低层协议建立之后才能进行更高层协议的连接，因此，首先要建立TCP连接，一般TCP连接的端口号是80。
2. Web浏览器向服务器发送请求命令。一旦建立了TCP连接，Web浏览器就会向Web服务器发送请求命令。例如：GET/sample/hello.jsp HTTP/1.1。
3. Web浏览器发送请求头信息。浏览器发送其请求命令之后，还要以头信息的形式向Web服务器发送一些别的信息，之后浏览器发送了一空白行来通知服务器，它已经结束了该头信息的发送。
4. Web服务器应答。 客户机向服务器发出请求后，服务器会客户机回送应答， HTTP/1.1 200 OK ，应答的第一部分是协议的版本号和应答状态码。
5. Web服务器发送应答头信息。 正如客户端会随同请求发送关于自身的信息一样，服务器也会随同应答向用户发送关于它自己的数据及被请求的文档。
6. Web服务器向浏览器发送数据。Web服务器向浏览器发送头信息后，它会发送一个空白行来表示头信息的发送到此为结束，接着，它就以Content-Type应答头信息所描述的格式发送用户所请求的实际数据。
7. Web服务器关闭TCP连接。一般情况下，一旦Web服务器向浏览器发送了请求数据，它就要关闭TCP连接，然后如果浏览器或者服务器在其头信息加入了这行代码：Connection:keep-alive

TCP连接在发送后将仍然保持打开状态，于是，浏览器可以继续通过相同的连接发送请求。保持连接节省了为每个请求建立新连接所需的时间，还节约了网络带宽。

## 推送心跳包是TCP包还是UDP包或者HTTP包

TCP

参考：

[Android推送技术研究](http://www.jianshu.com/p/584707554ed7)

[android 心跳的分析](http://blog.csdn.net/wangliang198901/article/details/16542567)

[Android微信智能心跳方案](http://mp.weixin.qq.com/s?__biz=MzAwNDY1ODY2OQ==&mid=207243549&idx=1&sn=4ebe4beb8123f1b5ab58810ac8bc5994&scene=4#wechat_redirect)

## Android 代码中实现 WAP 方式联网

- 通过 APN 列表，获取代码服务器和端口号，如果未设置，则设置成对应运营商的配置。
- 实现 HtppClient 代理。

## CMWAP, CMNET有何区别，网络通讯时是否要特殊处理？如何切换接入点？

## 断点续传

首先说多线程，我们要多线程下载一个大文件，就有开启多个线程，多个connection，既然是一个文件分开几个线程来下载，那肯定就是一个线程下载一个部分，不能重复。那么我们这么确定一个线程下载一部分呢，就需要我们在请求的header里面设置。

```java
conn.setRequestProperty("Range", "bytes="+startPos+"-"+endPos);  
```

这里startPos是指从数据端的哪里开始，endPos是指数据端的结束.根据这样我们就知道，只要多个线程，按顺序指定好开始跟结束，就可以不冲突的下载了。那么我们写文件的时候又该怎么写呢。

```java
byte[] buffer = new byte[1024];  
int offset = 0;  
print("Thread "+this.threadId+" starts to download from position "+startPos);  
RandomAccessFile threadFile = new RandomAccessFile(this.saveFile,"rwd");  
threadFile.seek(startPos);  
// ...  
threadFile.write(buffer,0,offset);  
```

这样就可以保证数据的完整性，也不会重复写入了。

那么我们接着说断点续传，断点续传其实也很简单，原理就是使用数据库保存上次每个线程下载的位置和长度。例如我开了两个线程T1，T2来下载一个文件，设文件总大小为1024M，那么就是每个线程下载512M。可是我的下载中断了，那么我下次启动线程的时候（继续下载），是不是应该要知道，我原来下载了多少呢。所以是这样的，我没下载一点，就更新数据库的数据，例如T1，下载了100M,就要实时更新数据库，记录下100M，并且记录下这个线程开始下载位置(startPos)，还有线程负责的长度(512M)。那么我继续下载的时候，就可以像服务器请求startPos+1000M开始的数据了，然后在文件里面也是seek（startPos+1000M）的位置继续下载，就可以实现断点续传了。

参考：

[android多线程断点续传原理解析](http://blog.csdn.net/crazy__chen/article/details/41947577)

## 网络的优化

参考：

[移动端网络优化](http://www.trinea.cn/android/mobile-performance-optimization/)

## HttpClient

# 动态加载

## 插件化，动态加载

参考：

[Android 插件化 动态升级](http://www.trinea.cn/android/android-plugin/)

## Android中ClassLoader和java中有什么关系和区别？

插件化的原理实际是 Java ClassLoader 的原理，看其他资料前请先看：[Java ClassLoader基础](http://www.trinea.cn/android/java-loader-common-class/)

Android 也有自己的 ClassLoader，分为 dalvik.system.DexClassLoader 和 dalvik.system.PathClassLoader，区别在于 PathClassLoader 不能直接从 zip 包中得到 dex，因此只支持直接操作 dex 文件或者已经安装过的 apk（因为安装过的 apk 在 cache 中存在缓存的 dex 文件）。而 DexClassLoader 可以加载外部的 apk、jar 或 dex文件，并且会在指定的 outpath 路径存放其 dex 文件。  

参考：

[Android 插件化 动态升级](http://www.trinea.cn/android/android-plugin/)

# 注解

## 什么是注解

参考：

[Java Annotation 及几个常用开源项目注解原理简析](http://www.trinea.cn/android/java-annotation-android-open-source-analysis/)

## 使用注解是否会影响性能

有些注解是用反射实现的所以影响性能。这类注解库对程序的性能影响并没有想象中的那么夸张，而且类似dagger2这类编译时注解的框架是没有性能影响的。

# JNI

## JNI开发流程



# WebView

## WebView和JS

## React Native

# jar

## 65k限制 做内部库设计时，最重要的考虑是jar的成本，方法数、体积。

Android 傻瓜式分包插件

GitHub：https://github.com/TangXiaoLv/Android-Easy-MultiDex
这是一个可自定义哪些类放在 MainDex 中的插件。ReadMe 中详细介绍了在使用 MultiDex 时，为了解决 MainDex 方法数超标的问题，碰到的一个个坑及如何解决，并列出了详细的参考资料，一篇很不错的文章。

参考：

[U8SDK——支持自动拆分成多个dex文件(MultiDex支持)](http://www.uustory.com/?p=2061)

# 其他

## APP启动过程



## 如何判断应用被强杀

在Applicatio中定义一个static常量，赋值为－1，在欢迎界面改为0，如果被强杀，application重新初始化，在父类Activity判断该常量的值。

参考：

http://blog.csdn.net/Small_Lee/article/details/51886746

## 应用被强杀如何解决

如果在每一个Activity的onCreate里判断是否被强杀，冗余了，封装到Activity的父类中，如果被强杀，跳转回主界面，如果没有被强杀，执行Activity的初始化操作，给主界面传递intent参数，主界面会调用onNewIntent方法，在onNewIntent跳转到欢迎页面，重新来一遍流程。

## 简述静默安装的原理，如何在无需root权限的情况下实现静默安装？

参考：

[Android常用代码之APK root权限静默安装](http://www.trinea.cn/android/android-silent-install/)

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

```java
@TargetApi(11) 
public void text() { 
if(Build.VERSION.SDK_INT >= 11){ 
	// 使用 API 11 的方法
} else {
	// 使用自己实现的方法
}
```

## 实现一个单例

```java
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

## 函数调用Trace 怎么玩

## AlarmManager以及Wakelock的使用

# 算法理解

## 什么是二分算法

# 设计模式

## Android 中主要用到的几种设计模式



## Android设计模式

参考：

http://blog.csdn.net/bboyfeiyu/article/details/44563871

# 架构设计


## mvc mvp mvvm

![](http://o9sn2y8lr.bkt.clouddn.com/16-10-19/71303594.jpg)

参考：

http://www.tianmaying.com/tutorial/AndroidMVC

## 你怎么看待在android上面应用MVC框架，是否有必要抽象独立于activity的C？

