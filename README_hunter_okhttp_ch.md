## OkHttp-Plugin

[English](https://github.com/Leaking/Hunter/blob/master/README_hunter_okhttp.md)

一个稍微上规模的项目，很大可能有很多个OkhttpClient散落在各个业务中，那么如果你想使用Interceptor/EventListener做网络监控，或者设置自定义Dns，
那就很麻烦，需要一处处为所有OkhttpClient设置Interceptor/EventListener/Dns，而且有些第三方依赖库里的OkhttpClient你是无能为力的。当然，你可以
反射去处理，但是这会遇到很多麻烦。

我在Okhtp提过类似问题的[issue](https://github.com/square/okhttp/issues/4228) 
 
OkHttp-Plugin 这个插件就可以帮忙你解决这个问题，让你两三句代码就可以为所有OkhttpClient设置全局的Interceptor/EventListener/Dns，方便你做进一步的网络监控、管理。

## 快速引入

在build.gradle中添加以下依赖

```groovy


dependencies {
    implementation 'com.quinn.hunter:hunter-okhttp-library:1.2.0'
}

repositories {
    maven {
        name = "GithubPackages"
        url = uri("https://maven.pkg.github.com/Leaking/Hunter")
        credentials {
            username = 'Leaking'
            password = '\u0067\u0068\u0070\u005f\u0058\u006d\u0038\u006e\u0062\u0057\u0031\u0053\u0053\u0042\u006a\u004a\u0064\u006f\u0071\u0048\u0064\u006b\u0036\u0034\u0077\u0031\u0054\u0066\u0074\u0071\u0052\u0046\u0068\u0042\u0032\u0047\u0057\u0037\u0046\u0070'
        }
    }
}

buildscript {
    repositories {
        maven {
            name = "GithubPackages"
            url = uri("https://maven.pkg.github.com/Leaking/Hunter")
            credentials {
                username = 'Leaking'
                password = '\u0067\u0068\u0070\u005f\u0058\u006d\u0038\u006e\u0062\u0057\u0031\u0053\u0053\u0042\u006a\u004a\u0064\u006f\u0071\u0048\u0064\u006b\u0036\u0034\u0077\u0031\u0054\u0066\u0074\u0071\u0052\u0046\u0068\u0042\u0032\u0047\u0057\u0037\u0046\u0070'
            }
        }
        google()
    }
    dependencies {
        classpath 'com.quinn.hunter:hunter-okhttp-plugin:1.2.0'
        classpath 'com.quinn.hunter:hunter-transform:1.2.0'
    }
}

apply plugin: 'hunter-okhttp'
    
```


```java

OkHttpHooker.installEventListenerFactory(CustomGlobalEventListener.FACTORY);
OkHttpHooker.installDns(new CustomGlobalDns());
OkHttpHooker.installInterceptor(new CustomGlobalInterceptor());
        
```
由于EventListener是okhttp 3.11才引入的，所以上面的使用方式需要okhttp3.11=及以上才行，如果你低于3.11，请查看这个wiki  [wiki for okhttp-below-3.11](https://github.com/Leaking/Hunter/wiki/Okhttp-below-3.11)。不过，升级okhttp的成本很低，建议还是把项目中的okhttp升级上来。