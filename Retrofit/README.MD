# Retrofit 

## 目录

+ [概述](#0)
+ [使用](#1)
+ [源码解析](#2)
+ [扩展知识](#3)


<h2 id="0"> 概述 </h2>

> [项目官网 (http://square.github.io/retrofit/)](http://square.github.io/retrofit/)               
  [项目源码：github (https://github.com/square/retrofit)](https://github.com/square/retrofit)

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
* path是相对路径，baseUrl是目录形式：              
  path = "apath"，baseUrl = "http://host:port/a/b/"
  Url = "http://host:port/a/b/apath"
* path是相对路径，baseUrl是文件形式：     
  path = "apath"，baseUrl = "http://host:port/a/b"
  Url = "http://host:port/a/apath"
* path是完整的 Url：
  path = "http://host:port/aa/apath"，baseUrl = "http://host:port/a/b"
  Url = "http://host:port/aa/apath"

建议采用第二种方式来配置，并尽量使用同一种路径形式。如果你在代码里面混合采用了多种配置形式，恰好赶上你哪天头晕眼花，信不信分分钟写一堆 bug 啊哈哈。


#### 2.2 参数类型
发请求时，需要传入参数，Retrofit通过注解的形式令 Http 请求的参数变得更加直接，而且类型安全。

##### 2.2.1 Query & QueryMap
```
@GET("/list")
Call<ResponseBody> list(@Query("page") int page);
```
Query其实就是 Url 中 ‘?’ 后面的 key-value。
这时候你肯定想，如果我有很多个 Query，这么一个个写岂不是很累？而且根据不同的情况，有些字段可能不传，这与方法的参数要求显然也不相符。于是，打群架版本的QueryMap横空出世了，使用方法很简单，我就不多说了。

##### 2.2.2 Field & FieldMap
其实我们用POST的场景相对较多，绝大多数的服务端接口都需要做加密、鉴权和校验，GET显然不能很好的满足这个需求。使用POST提交表单的场景就更是刚需了，怎么提呢？
```
@FormUrlEncoded
@POST("/") 
Call<ResponseBody> example( @Field("name") String name, @Field("occupation") String occupation);
```
其实也很简单，我们只需要定义上面的接口就可以了，我们用 Field声明了表单的项，这样提交表单就跟普通的函数调用一样简单直接了。
等等，你说你的表单项不确定个数？还是说有很多项你懒得写？Field同样有个打群架的版本——FieldMap，赶紧试试吧~~


##### 2.2.3  Part & PartMap
这个是用来上传文件的。话说当年用 HttpClient上传个文件老费劲了，一会儿编码不对，一会儿参数错误（也怪那时段位太低吧TT）。。。可是现在不同了，自从有了Retrofit，妈妈再也不用担心文件上传费劲了~~~
```
public interface FileUploadService {
  @Multipart 
  @POST("upload") 
  Call<ResponseBody> upload(@Part("description") RequestBody description, @Part MultipartBody.Part file); 
}
```
如果你需要上传文件，和我们前面的做法类似，定义一个接口方法，需要注意的是，这个方法不再有 @FormUrlEncoded这个注解，而换成了@Multipart，后面只需要在参数中增加Part就可以了。也许你会问，这里的Part和Field究竟有什么区别，其实从功能上讲，无非就是客户端向服务端发起请求携带参数的方式不同，并且前者可以携带的参数类型更加丰富，包括数据流。也正是因为这一点，我们可以通过这种方式来上传文件，下面我们就给出这个接口的使用方法：
```
// 先创建service
FileUploadService service = retrofit.create(FileUploadService.class);

// 构建要上传的文件
File file = new File(filename);
RequestBody requestFile = RequestBody.create(MediaType.parse("application/otcet-stream", file));
```




<h2 id="2"> 源码解析 </h2>

<h2 id="3"> 扩展知识 </h2>
