# Retrofit 

## 目录

+ [概述](#0)
+ [使用](#1)
+ [源码解析](#2)
+ [扩展知识](#3)


<h2 id="0"> 概述 </h2>

> [项目官网 (http://square.github.io/retrofit/)](http://square.github.io/retrofit/)
>                    [项目源码：github (https://github.com/square/retrofit)](https://github.com/square/retrofit)

**Retrofit**是一个 [RESTful](http://baike.baidu.com/link?url=diNiB-Mag1_w58m9Ygv1Q_exrnWeIwysrGakOZpcB1gnhiNfqhX-5zUgM6r4lsU2eilk7J7Qcg7EJdXp7K2M8K "RESTful百度百科") 的 HTTP 网络请求框架的**封装**。注意这里并没有说它是`网络请求框架`，主要原因在于网络请求的工作并不是Retrofit来完成的。Retrofit2.0 开始内置[OkHttp "http://square.github.io/okhttp/" ](http://square.github.io/okhttp/)( `OkHttp` 是基于http协议封装的一套请求客户端，虽然它也可以开线程，但根本上它更偏向真正的请求，跟`HttpClient`, `HttpUrlConnection`的职责是一样的)，前者专注于接口的封装，后者专注于网络请求的高效，二者分工协作。

![Retrofit架构图](pictures/pic1.jpg)

我们的应用程序通过Retrofit请求网络，实际上是使用Retrofit接口层封装请求参数、Header、Url 等信息，之后由OkHttp完成后续的请求操作，在服务端返回数据之后，OkHttp将原始的结果交给Retrofit，后者根据用户的需求对结果进行解析的过程。


<h2 id="1"> 使用 </h2>

### 1、引用依赖
Android studio开发项目引用Retrofit需要在 build.gradle 脚本中添加依赖：
> compile 'com.squareup.retrofit2:retrofit:2.1.0'

### 2、Retrofit各接口概念一览

Retrofit要求将你的HTTP访问API接口都定义成Java接口，例如，定义一个GET请求接口，定义如下：
```
public interface GitHubService {
  @GET("users/{user}/repos")
  Call<List<Repo>> listRepos(@Path("user") String user);
}
```
当调用listRepos接口发起请求时，{user}会被替换为方法的第一个参数user。

请求接口定义好了，接下来需要进行Retrofit初始化了：
```
Retrofit retrofit = new Retrofit.Builder()
    .baseUrl("https://api.github.com/")
    .build();

GitHubService service = retrofit.create(GitHubService.class);
```

然后就可以通过创建好的实例化对象进行请求操作咯：
```
Call<List<Repo>> repos = service.listRepos("octocat");
```

发请求的代码就像前面这一句，返回的 repos其实并不是真正的数据结果，它更像一条指令，你可以在合适的时机去执行它：
```
// 同步调用
List<Repo> data = repos.execute();

// 异步调用
repos.enqueue(new Callback<List<Repo>>(){
          @Override
          public void onResponse(Call<List<Repo>> call, Response<List<Repo>> response) {
              List<Repo> data = response.body();
          }
          
          @Override
          public void onFailure(Call<List<Repo>> call, Throwable t) {
              t.printStackTrace();
          }
    });
```
啥感觉？有没有突然觉得请求接口就好像访问自家的方法一样简单？呐，前面我们看到的，就是 Retrofit官方的 demo 了。你以为这就够了？噗~怎么可能。

#### 2.1 Url配置

Retrofit支持的协议包括**GET/POST/PUT/DELETE/HEAD/PATCH**，当然你也可以直接用HTTP来自定义请求。这些协议均以[注解]("http://baike.baidu.com/link?url=sA6pIs6zkD6FWOSnN_AlYauX0OXcCRhN0Nk22G3BrmckOralM9jSOKKKgboKv1gUFA339BnCkh6ikBGZPXkWYa" "注解百度百科")的形式进行配置，比如我们已经见过GET的用法：
```
@GET("users/{user}/repos")
Call<List<Repo>> listRepos(@Path("user") String user);
```
这些注解都有一个参数 value，用来配置其路径，比如示例中的 **users/{user}/repos**，我们还注意到在构造Retrofit之时我们还传入了一个**baseUrl("https://api.github.com/")**，请求的完整 Url 就是通过**baseUrl**与**注解的value（下面称 “path“ ）** 整合起来的，具体整合的规则如下：
* path是绝对路径的形式：  
  path = "/apath"，baseUrl = "http://host:port/a/b"     
  Url = "http://host:port/apath"
* sdf 




<h2 id="2"> 源码解析 </h2>

<h2 id="3"> 扩展知识 </h2>
