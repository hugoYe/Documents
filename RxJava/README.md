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
```
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





<h3 id="3">API 介绍和原理简析</h3>

<h4 id="3.1">1. 概念：扩展的观察者模式</h4>

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

