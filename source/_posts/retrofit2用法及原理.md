---
title: retrofit2用法及原理
date: 2017-01-17 17:17:53
tags: [android]
---
retrofit2 官网 http://square.github.io/retrofit/

<!-- more -->

####首先介绍下retrofit2的使用
- 1.创建retrofit的对象
```
Retrofit retrofit = new Retrofit.Builder()        
         .baseUrl(“http://demo.com/”)       
         .addConverterFactory(GsonConverterFactory.create())    
         .addCallAdapterFactory(RxJavaCallAdapterFactory.create())  
         .client(client)
         .build();
```
- 2.创建Service接口
```
    /** * Created by wangliang on 16-9-28. */
    public interface DemoService {   
         @FormUrlEncoded    
         @POST("api") //注意Observable为与Rxjava结合（后边文章会介绍TODO）
         Observable<LoginResp> login(@FieldMap Map<String, String> params);
    }
```
    这里涉及到注解参数可参考官网
    - get请求 @GET  
    - post请求 @POST
    - PUT请求 @PUT
    - DELETE请求 @DELETE

    ######url带动态参数
    ```
    @GET("group/{id}/users")
    Call<List<User>> groupList(@Path("id") int groupId);
    ```
    这样调用时groupId=1，则url为baseurl/group/1/users
    ######get请求参数
   ```
   @GET("group/{id}/users")
   Call<List<User>> groupList(@Path("id") int groupId, @Query("sort") String sort);
   ```
   这样调用时group=1并且sort="aaa"， 则url为baseurl/group/1/users?sort=aaa
   ######get多参数可采用map传递
   ```
   @GET("group/{id}/users")
   Call<List<User>> groupList(@Path("id") int groupId,
                                             @QueryMap Map<String, String> options);
   ```
   ######form表单形式提交
   ```
   @FormUrlEncoded
   @POST("user/edit")
   Call<User> updateUser(@Field("first_name") String first,
                                            @Field("last_name") String last);
   ```
   ######form表单多复杂参数提交（如上边例子的代码）
   ```
   @FormUrlEncoded    
   @POST("api")
   Observable<LoginResp> login(@FieldMap Map<String, String> params);
   ```

   ######retrofit文件上传会另外做介绍，这里不做细说
- 3.实例化Service对象
```
mDemoAPIService = retrofit.create(DemoService.class);
```
   注意：这里最好和retrofit放在一起，比如放在一个单例对象中（个人建议）
- 4.实际调用
```
public void login(String mobile, String pwd, MySubscriber<PtLoginResp> subscriber) {
    HashMap map = new HashMap();
    map.put("mobile",mobile);
    map.put("pwd", pwd);
    Observable<LoginResp> o = mDemoAPIService.login(map);//这里集成了Rxjava
    o.map(func1).subscribeOn(Schedulers.io()) .unsubscribeOn(Schedulers.io())
         .observeOn(AndroidSchedulers.mainThread()) .subscribe(subscriber);
}
```
######原生retrofit调用方法形如
```
Call call = service.login();
call.execute();//同步执行请求
call.enqueue();//异步执行请求
//请求取消
可以在onPause onStop 或onDestroy中添加
if(call.isCanceled()) {
      call.cancel()
}
```
######Rxjava调用（这里是异步调用）
```

   o.map(func1).subscribeOn(Schedulers.io())
        .unsubscribeOn(Schedulers.io())
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe(subscriber);
```
######注意这里subscriber我封装了一个抽象类 上层调用需要传递一个实现类
```
/** * Created by wangliang on 16-9-29. */
public abstract class MySubscriber<T> extends Subscriber<T> {
    @Override
    public void onCompleted() {
        // do nothing.    
    }
    @Override
    public void onError(Throwable e) {
        if(e instanceof ApiException) {
            onError(((ApiException) e).getErrorMsg());
        } else {
            onError("请求发生错误");
        }
    }
    @Override
    public void onNext(T t) {
        onSucc(t);
    }
    public abstract void onSucc(T t);
    public abstract void onError(String error);}
```
最上层调用代码如下
```
login("13800138000", "123456", new MySubscriber<PtLoginResp>() {
    @Override
    public void onSucc(LoginResp loginResp) {
        Log.d("WLTest", "token:" + loginResp.getToken() + ", userid:" +
                              loginResp.getUserGuid());
    }
    @Override
    public void onError(String error) {
        Log.d("WLTest", error);
    }
});
```
####到此Retrofit用法基本介绍完毕
--------
####retrofit原理分析
- 首先介绍对应Retrofit的几个关键接口
 - Callback<T>
 ```
 public interface Callback<T> {
      void onResponse(Call<T> call, Response<T> response);  
      void onFailure(Call<T> call, Throwable t);
}
 ```
这个接口就是retrofit请求数据返回的接口
 - Converter<F, T>
```
public interface Converter<F, T> {
       T convert(F value) throws IOException;
}
```
这个接口主要的作用就是将HTTP返回的数据解析成Java对象，主要有Xml、Gson、
protobuf等等，你可以在创建Retrofit对象时添加你需要使用的Converter实现
 - Call<T>
 ```
 public interface Call<T> extends Cloneable {
       Response<T> execute() throws IOException;
       void enqueue(Callback<T> callback);
       boolean isExecuted();
       void cancel();
       boolean isCanceled();
       Call<T> clone();
       Request request();
 }
 ```
这个接口主要的作用就是发送一个HTTP请求，Retrofit默认的实现是OkHttpCall<T>
，你可以根据实际情况实现你自己的Call类
 - CallAdapter<T>
```
    public interface CallAdapter<T> {
        Type responseType();
        <R> T adapt(Call<R> call);
    }
```
######这个接口的作用就是将Call对象转换成另一个对象
这个接口的实现类都跟Rxjava有关，
CompletableCallAdapter 将 Call转换成Completable
ResponseCallAdapter 将Call<R>转换成Observable<Response<R>>
ResultCallAdapter  将Call<R>转换成Observable<Result<R>>
SimpleCallAdapter 将Call<R>转换成Observable<R>
这样通过适配器模式可以很好的支持Rxjava
- 接下来看retrofit Builder的build方法
```
public Retrofit build() {
    if (baseUrl == null) {
      throw new IllegalStateException("Base URL required.");
    }
    okhttp3.Call.Factory callFactory = this.callFactory;
    //这里默认用okhttp3
    if (callFactory == null) {
      callFactory = new OkHttpClient();
    }

   //线程池配置
    Executor callbackExecutor = this.callbackExecutor;
    if (callbackExecutor == null) {
      callbackExecutor = platform.defaultCallbackExecutor();
    }
    // Make a defensive copy of the adapters and add the default Call adapter.
    List<CallAdapter.Factory> adapterFactories = new ArrayList<>(this.adapterFactories);
    adapterFactories.add(platform.defaultCallAdapterFactory(callbackExecutor));
    // Make a defensive copy of the converters.
    List<Converter.Factory> converterFactories = new ArrayList<>(this.converterFactories);
    return new Retrofit(callFactory, baseUrl, converterFactories, adapterFactories,
         callbackExecutor, validateEagerly);
}
```
该方法主要是构建retrofit基本参数
baseUrl      API Root
converterFactories 数据序列化反序列化工厂 如Gson 等
- 其次看Retrofit.create方法
这个可以说是retrofit的核心方法
```
public <T> T create(final Class<T> service) {
     Utils.validateServiceInterface(service);
     if (validateEagerly) {
         eagerlyValidateMethods(service);
     }
     return (T) Proxy.newProxyInstance(service.getClassLoader(), new Class<?>[] { service },
      new InvocationHandler() {
        private final Platform platform = Platform.get();
        @Override
        public Object invoke(Object proxy, Method method, Object... args)
           throws Throwable {
          // If the method is a method from Object then defer to normal invocation.     
          if (method.getDeclaringClass() == Object.class) {
            return method.invoke(this, args);
          }
          if (platform.isDefaultMethod(method)) {
            return platform.invokeDefaultMethod(method, service, proxy, args);
          }
          ServiceMethod serviceMethod = loadServiceMethod(method);
          OkHttpCall okHttpCall = new OkHttpCall<>(serviceMethod, args);
          return serviceMethod.callAdapter.adapt(okHttpCall);
        }
      });
}
```
该方法就是一个动态代理（可参考 http://www.jianshu.com/p/24d880a0b573 ），这样我们在上边接口中定义的一些变量就可以在执行真正请求之前做替换了
```ServiceMethod```可以称为核心处理器，传入Retrofit对象和Method对象，调用各个接口和解析器，最终生成一个Request，包含api 的域名、path、http请求方法、请求头、是否有body、是否是multipart等等。最后返回一个Call对象，Retrofit2中Call接口的默认实现是OkHttpCall，它默认使用```OkHttp3```作为底层http请求client 这个在上边的源码中可以看到
- 下面来看构建ServiceMethod对象
```
// 首先获取以下对象
callAdapter = createCallAdapter();  //CallAdapter
responseType = callAdapter.responseType(); //response Type
responseConverter = createResponseConverter(); //Converter
```
```
//其次解析注解 主要就是获取Http请求的方法，比如是GET还是POST还是其他形式，如果
//没有，程序就会报错，还会做一系列的检查
for (Annotation annotation : methodAnnotations) {    
    parseMethodAnnotation(annotation);
}
if (httpMethod == null) {  
    throw methodError("HTTP method annotation is required (e.g., @GET, @POST, etc.).");
}
if (!hasBody) {
    if (isMultipart) {
      throw methodError(        "Multipart can only be specified on HTTP methods with   request body (e.g., @POST).");
    }
    if (isFormEncoded) {
      throw methodError("FormUrlEncoded can only be specified on HTTP methods with "        + "request body (e.g., @POST).");
    }
}
```
```
//替换参数等占位符
//比如上面api中带有一个参数{user} ，这是一个占位符，而真实的参数值在Java方法
//中传入，那么Retrofit会使用一个ParameterHandler来进行替换
int parameterCount = parameterAnnotationsArray.length;
parameterHandlers = new ParameterHandler<?>[parameterCount];
for (int p = 0; p < parameterCount; p++) {
     Type parameterType = parameterTypes[p];
     if (Utils.hasUnresolvableType(parameterType)) {
        throw parameterError(p, "Parameter type must not include a type variable or wildcard: %s",        parameterType);
     }
     Annotation[] parameterAnnotations = parameterAnnotationsArray[p];
     if (parameterAnnotations == null) {
        throw parameterError(p, "No Retrofit annotation found.");
     }
     parameterHandlers[p] = parseParameter(p, parameterType, parameterAnnotations);
}
```
- 请求的执行
OkHttpCall 是实现了Call 接口的，并且是真正调用OkHttp3 发送Http请求的类。 OkHttp3 发送一个Http请求需要一个Request对象，而这个Request对象就是从ServiceMethod的toRequest 返回的 （okhttp3 会在以后介绍）
总结一句话
######OkHttpCall就是调用ServiceMethod获得一个可以执行的Request对象，然后等到Http请求返回后，再将response body传入ServiceMethod中，ServiceMethod就可以调用Converter接口将response body转成一个Java对象

#到此Retrofit原理主要的都记录在此，如有遗漏，欢迎前方高能指教～～
