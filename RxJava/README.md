# RxJava 学习笔记

### 目录索引
* [RxJava 到底是什么](#1)

### RxJava 到底是什么
<h3 </h3>
一个词：__异步__。

RxJava 在 GitHub 主页上的自我介绍是 "a library for composing asynchronous and event-based programs using observable sequences for the Java VM"（一个在 Java VM 上使用可观测的序列来组成异步的、基于事件的程序的库）。这就是 RxJava ，概括得非常精准。

然而，对于初学者来说，这太难看懂了。因为它是一个『总结』，而初学者更需要一个『引言』。

其实， RxJava 的本质可以压缩为异步这一个词。说到根上，它就是一个实现异步操作的库，而别的定语都是基于这之上的。

### RxJava 好在哪
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
