# Android 源码设计模式解析与实战学习笔记

## MVC模式的介绍

`MVC` 全称是 “Model-View-Controller”，也就是模型——视图——控制器，它是一种`框架模式`而非`设计模式`，`GOF`把`MVC`看作是3种设计模式：观察者模式、策略模式与组合模式的合体，而且其核心在观察者模式，也就是一个基于`发布/订阅者模型`的框架。

常见的`框架模式`还有：MVVC、MTV、CBD、ORM、MVP等。

`框架模式`与`设计模式`的不同之处简而言之就是：框架是大智慧，用来对软件设计进行分工；设计模式是小技巧，对具体问题提出解决方案，以提高代码复用率，降低耦合度。 

`MVC` 框架模式在Android中的应用实例：
Android中对于UI部分的开发，将Activity作为Controller，连接Model和View之间的状态。


## 走向灵活软件之路——面向对象的六大原则

### 1、优化代码的第一步——单一职责原则
英文名称是：Single Responsibility Principle，缩写SRP。

### 2、让程序更稳定、更灵活——开闭原则
英文名称是：Open Close Principle，缩写是OCP。    
软件中的对象（类、模块、函数等）应该对于扩展是开放的，但是，对于修改是封闭的。而遵循开闭原则的重要手段应该是通过抽象。

### 3、构建扩展性更好的系统——里氏替换原则
英文名称是：Liskov Substitution Principle，缩写是LSP。   
里氏替换原则的核心原理是抽象，抽象又依赖于继承这个特性。

### 4、让项目拥有变化的能力——依赖倒置原则
英文名称是：Dependence Inversion Principle，缩写是DIP。  
在Java语言中，抽象就是指接口或抽象类，两者都是不能直接被实例化的；细节就是实现类，实现接口或者抽象类产生的类就是细节，其特点就是，可以直接被实例化，也就是可以加上一个关键字new产生一个对象。     

高层模块就是调用端，低层模块就是具体实现类。依赖倒置原则在Java中的表现就是：**模块间的依赖通过抽象发生，实现类之间不发生直接的依赖关系，其依赖关系是通过接口或者抽象类产生的。**

### 5、系统有更高的灵活性——接口隔离原则
英文名称是：Interface Segregation Principle，缩写是ISP。

### 6、更好的可扩展性——迪米特原则
英文名称是：Law of Demeter，缩写是LOD。也称为最少只是原则（Least Knowledge Principle）。  
一个对象应该对其他对象有最少的了解，通俗的讲，一个类应该对自己需要耦合或调用的类知道的最少，类的内部如何实现与调用者或者依赖者没关系，调用者或者依赖者只需要知道它需要的方法即可，其他的可一概不管。


## 应用最广的模式——单例模式
### 1、饿汉单例模式
```
public class Singleton {
	private static final instance = new Singleton();

	private Singleton() {}

	public static Singleton getInstance() {
		return instance;
	}
}
```

### 2、懒汉单例模式
```
public class Singleton {
	private static Singleton instance;

	private Singleton() {}

	public static synchronized Singleton getInstance() {
		if (instance == null) {
			instance = new Singleton();
		}
		return instance;
	}
}
```       

优点：单例只有在使用时才会被实例化，在一定程度上节约了资源。   
缺点：第一次加载时需要及时进行实例化，反应稍慢，最大的问题是每次调用getInstance都进行同步，造成不必要的同步开销。

一般不建议使用**懒汉模式**。

### 3、Double Check Lock（DCL）实现单例
```
public class Singleton {
	private volatile static Singleton instance = null;

	private Singleton() {}

	public static Singleton getInstance() {
		if (instance == null) {
			synchronized (Singleton.class) {
				if (instance == null) {
					instance = new Singleton();
				}
			}
		}
		return instance;
	}
}
```      

优点：资源利用率高，第一次执行getInstance时单例对象才会被实例化，效率高。     
缺点：第一次加载时反应慢，也由于Java内存模型的原因偶尔会失败。在高并发环境下也有一定的缺陷，虽然发生的概率很小。         

DCL模式是使用最多的单例实现方式，它能够在需要时才实例化单例对象，并且能够在绝大多数场景下保证单例对象的唯一性，除非你的代码在并发场景比较复杂或者低于JDK6版本下使用，否则，这种方式一般能够满足需求。

### 4、静态内部类单例模式
```
public class Singleton {
	private Singleton() {}

	public static Singleton getInstance() {
		return SingletonHolder.sInstance;
	}

	// 静态内部类
	private static class SingletonHolder {
		private static final Singleton sInstance = new Singleton();
	}
}
```

被推荐使用的单例模式。

### 5、使用容器实现单例模式
```
public class SingletonManager {
	private static Map<String, Object> objMap = new 	HashMap<String, Object>();

	private SingletonManager() {}

	public static void registerService(String key, Object 	instance) {
		if(!objMap.containsKey(key)) {
			objMap.put(key, instance);
		}
	}

	public static Object getService(String key) {
		return objMap.get(key);
	}
}
```

## 自由扩展你的项目——Builder模式
### Builder模式定义
将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。

### Builder模式的使用场景
1.相同的方法，不同的执行顺序，产生不同的事件结果时。
2.多个部件或零件，都可以装配到一个对象中，但是产生的运行结果又不相同时。
3.产品类非常复杂，或者产品类中的调用顺序不同产生了不同的作用，这个时候使用建造者模式非常合适。
4.当初始化一个对象特别复杂，如参数多，且很多参数都具有默认值时。