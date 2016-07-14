# RxJava 学习笔记

### 目录索引
* [RxJava 到底是什么](#1)
* [RxJava 好在哪](#2)
* [API 介绍和原理简析](#3)
    + [1. 概念：扩展的观察者模式](#3.1)
        - [观察者模式](#3.1.1)
        - [RxJava 的观察者模式](#3.1.2)
    
    + [2. 基本实现](#3.2)
        - [1) 创建 Observer](#3.2.1)
        - [2) 创建 Observable](#3.2.2)
        - [3) Subscribe (订阅)](#3.2.3)
        - [4) 场景示例](#3.2.4)
        
    + [3. 线程控制 —— Scheduler (一)](#3.3)
        - [1) Scheduler 的 API (一)](#3.3.1)
        - [2) Scheduler 的原理 (一)](#3.3.2)
        
    + [4. 变换](#3.4)
        - [1) API](#3.4.1)
        - [2) 变换的原理：lift()](#3.4.2)
        - [3) compose: 对 Observable 整体的变换](#3.4.3)
        
    + [5. 线程控制：Scheduler (二)](#3.5)
        - [1) Scheduler 的 API (二)](#3.5.1)
        - [2) Scheduler 的原理（二）](#3.5.2)
        - [3) 延伸：doOnSubscribe()](#3.5.3)
        
* [RxJava 的适用场景和使用方式](#4)
    + [1. 与 Retrofit 的结合](#4.1)
    + [2. RxBinding](#4.2)
    + [3. 各种异步操作](#4.3)
    + [4. RxBus](#4.4)
    
* [最后](#5)


<h3 id="1">RxJava 到底是什么</h3>
一个词：__异步__。

RxJava 在 GitHub 主页上的自我介绍是 "a library for composing asynchronous and event-based programs using observable sequences for the Java VM"（一个在 Java VM 上使用可观测的序列来组成异步的、基于事件的程序的库）。这就是 RxJava ，概括得非常精准。

然而，对于初学者来说，这太难看懂了。因为它是一个『总结』，而初学者更需要一个『引言』。

其实， RxJava 的本质可以压缩为异步这一个词。说到根上，它就是一个实现异步操作的库，而别的定语都是基于这之上的。


<h3 id="2">RxJava 好在哪</h3>
换句话说，『同样是做异步，为什么人们用它，而不用现成的 AsyncTask / Handler / XXX / ... ？』

一个词：__简洁__。

异步操作很关键的一点是程序的简洁性，因为在调度过程比较复杂的情况下，异步代码经常会既难写也难被读懂。 Android 创造的 ```AsyncTask``` 和```Handler``` ，其实都是为了让异步代码更加简洁。RxJava的优势也是简洁，但它的简洁的与众不同之处在于，**随着程序逻辑变得越来越复杂，它依然能够保持简洁**。

　　　　　　　　　　　　　                   ![举个例子](http://ww4.sinaimg.cn/large/52eb2279jw1f2rx409pcnj2044048mx5.jpg "title text")

假设有这样一个需求：界面上有一个自定义的视图 ```imageCollectorView```，它的作用是显示多张图片，并能使用 ```addImage(Bitmap)``` 方法来任意增加显示的图片。现在需要程序将一个给出的目录数组 ```File[] folders``` 中每个目录下的 png 图片都加载出来并显示在 ```imageCollectorView```中。需要注意的是，由于读取图片的这一过程较为耗时，需要放在后台执行，而图片的显示则必须在 UI 线程执行。常用的实现方式有多种，我这里贴出其中一种：
```android
new Thread() {
    @Override
    public void run() {
        super.run();
        for (File folder : folders) {
            File[] files = folder.listFiles();
            for (File file : files) {
                if (file.getName().endsWith(".png")) {
                    final Bitmap bitmap = getBitmapFromFile(file);
                    getActivity().runOnUiThread(new Runnable() {
                        @Override
                        public void run() {
                            imageCollectorView.addImage(bitmap);
                        }
                    });
                }
            }
        }
    }
}.start();
```

而如果使用 RxJava ，实现方式是这样的：

```java
Observable.from(folders)
    .flatMap(new Func1<File, Observable<File>>() {
        @Override
        public Observable<File> call(File file) {
            return Observable.from(file.listFiles());
        }
    })
    .filter(new Func1<File, Boolean>() {
        @Override
        public Boolean call(File file) {
            return file.getName().endsWith(".png");
        }
    })
    .map(new Func1<File, Bitmap>() {
        @Override
        public Bitmap call(File file) {
            return getBitmapFromFile(file);
        }
    })
    .subscribeOn(Schedulers.io())
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe(new Action1<Bitmap>() {
        @Override
        public void call(Bitmap bitmap) {
            imageCollectorView.addImage(bitmap);
        }
    });
```
那位说话了：『你这代码明明变多了啊！简洁个毛啊！』大兄弟你消消气，我说的是逻辑的简洁，不是单纯的代码量少（逻辑简洁才是提升读写代码速度的必杀技对不？）。观察一下你会发现， RxJava 的这个实现，是一条从上到下的**链式调用**，没有任何嵌套，这在逻辑的简洁性上是具有优势的。当需求变得复杂时，这种优势将更加明显（试想如果还要求只选取前 10 张图片，常规方式要怎么办？如果有更多这样那样的要求呢？再试想，在这一大堆需求实现完两个月之后需要改功能，当你翻回这里看到自己当初写下的那一片`迷之缩进`，你能保证自己将迅速看懂，而不是对着代码重新捋一遍思路？）。

另外，如果你的 IDE 是 Android Studio ，其实每次打开某个 Java 文件的时候，你会看到被自动 **Lambda** 化的预览，这将让你更加清晰地看到程序逻辑：
```java
Observable.from(folders)
    .flatMap((Func1) (folder) -> { Observable.from(file.listFiles()) })
    .filter((Func1) (file) -> { file.getName().endsWith(".png") })
    .map((Func1) (file) -> { getBitmapFromFile(file) })
    .subscribeOn(Schedulers.io())
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe((Action1) (bitmap) -> { imageCollectorView.addImage(bitmap) });
```

> 如果你习惯使用 Retrolambda ，你也可以直接把代码写成上面这种简洁的形式。而如果你看到这里还不知道什么是 Retrolambda ，我不建议你现在就去学习它。原因有两点：1. Lambda 是把双刃剑，它让你的代码简洁的同时，降低了代码的可读性，因此同时学习 RxJava 和 Retrolambda 可能会让你忽略 RxJava 的一些技术细节；2. Retrolambda 是 Java 6/7 对 Lambda 表达式的非官方兼容方案，它的向后兼容性和稳定性是无法保障的，因此对于企业项目，使用 Retrolambda 是有风险的。所以，与很多 RxJava 的推广者不同，我并不推荐在学习 RxJava 的同时一起学习 Retrolambda。事实上，我个人虽然很欣赏 Retrolambda，但我从来不用它。

在Flipboard 的 Android 代码中，有一段逻辑非常复杂，包含了多次内存操作、本地文件操作和网络操作，对象分分合合，线程间相互配合相互等待，一会儿排成人字，一会儿排成一字。如果使用常规的方法来实现，肯定是要写得欲仙欲死，然而在使用 RxJava 的情况下，依然只是一条链式调用就完成了。它很长，但很清晰。

所以， RxJava 好在哪？就好在简洁，好在那把什么复杂逻辑都能穿成一条线的简洁。


<h3 id="3">API 介绍和原理简析</h3>

这个我就做不到一个词说明了……因为这一节的主要内容就是一步步地说明 RxJava 到底怎样做到了异步，怎样做到了简洁。


<h4 id="3.1">1. 概念：扩展的观察者模式</h4>

RxJava 的异步实现，是通过一种扩展的观察者模式来实现的。

观察者模式

先简述一下观察者模式，已经熟悉的可以跳过这一段。

观察者模式面向的需求是：A 对象（观察者）对 B 对象（被观察者）的某种变化高度敏感，需要在 B 变化的一瞬间做出反应。举个例子，新闻里喜闻乐见的警察抓小偷，警察需要在小偷伸手作案的时候实施抓捕。在这个例子里，警察是观察者，小偷是被观察者，警察需要时刻盯着小偷的一举一动，才能保证不会漏过任何瞬间。程序的观察者模式和这种真正的『观察』略有不同，观察者不需要时刻盯着被观察者（例如 A 不需要每过 2ms 就检查一次 B 的状态），而是采用注册(Register)或者称为订阅(Subscribe)的方式，告诉被观察者：我需要你的某某状态，你要在它变化的时候通知我。 Android 开发中一个比较典型的例子是点击监听器 OnClickListener 。对设置 OnClickListener 来说， View 是被观察者， OnClickListener 是观察者，二者通过 setOnClickListener() 方法达成订阅关系。订阅之后用户点击按钮的瞬间，Android Framework 就会将点击事件发送给已经注册的 OnClickListener 。采取这样被动的观察方式，既省去了反复检索状态的资源消耗，也能够得到最高的反馈速度。当然，这也得益于我们可以随意定制自己程序中的观察者和被观察者，而警察叔叔明显无法要求小偷『你在作案的时候务必通知我』。

OnClickListener 的模式大致如下图：

![图 1](/pictures/pic1.jap)






<h5 id="3.1.1">观察者模式</h5>

<h5 id="3.1.2">RxJava 的观察者模式</h5>


<h4 id="3.2">2. 基本实现</h4>

<h5 id="3.2.1">1) 创建 Observer</h5>

<h5 id="3.2.2">2) 创建 Observable</h5>

<h5 id="3.2.3">3) Subscribe (订阅)</h5>

<h5 id="3.2.4">4) 场景示例</h5>


<h4 id="3.3">3. 线程控制 —— Scheduler (一)</h4>

<h5 id="3.3.1">1) Scheduler 的 API (一)</h5>

<h5 id="3.3.2">2) Scheduler 的原理 (一)</h5>


<h4 id="3.4">4. 变换</h4>

<h5 id="3.4.1">1) API</h5>

<h5 id="3.4.2">2) 变换的原理：lift()</h5>

<h5 id="3.4.3">3) compose: 对 Observable 整体的变换</h5>


<h4 id="3.5">5. 线程控制：Scheduler (二)</h4>

<h5 id="3.5.1">1) Scheduler 的 API (二)</h5>

<h5 id="3.5.2">2) Scheduler 的原理（二）</h5>

<h5 id="3.5.3">3) 延伸：doOnSubscribe()</h5>


<h3 id="4">RxJava 的适用场景和使用方式</h3>


<h4 id="4.1">1. 与 Retrofit 的结合</h4>

<h4 id="4.2">2. RxBinding</h4>

<h4 id="4.3">3. 各种异步操作</h4>

<h4 id="4.4">4. RxBus</h4>

<h3 id="5">最后</h3>

