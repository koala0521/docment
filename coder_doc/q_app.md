# 轻粒子快应用统计平台 SDK 接入指南

---

##### 使用范围：

本文档适用于最新版 `SDK` ，如果您的 `SDK` 版本较低，建议升级最新版 `SDK` （[点击下载 SDK ][2]）。

SDK 版本号：`sdk_version 1.3.2.0`

发布日期：`2018-12-21`


![Alt text](/SDK_IMG.jpg)

### 1.申请app_key

注册账号并登录 [轻粒子官网][1] 在“我的快应用”页面点击创建快应用，成功创建快应用后即可获取到对应快应用 app_key。


### 2.接入 SDK

#### 2.1 下载SDK

下载 SDK文件并解压，将其中的`appStatistics.min.js、statistics.config.js`拷贝到`app.ux`同级的目录（`src`目录下）。
下载地址： [轻粒子快应用统计平台 SDK 下载]
(http://www.qinglizi.cn/quickapp_statistics_sdk.zip)


#### 2.2 SDK 导入项目

把 `sdk` 中的 `appStatistics.min.js、statistics.config.js` 拷贝到项目中和 `app.ux` 同级的目录。


#### 2.3 配置 app_key

用编辑器打开 `statistics.config.js` ，填入从官网获取的 `app_key` ：

```javascript

    export default {
    'app_key' : '0000000000000'   //请在此行填写统计平台获取的 app_kye
    }

```

#### 2.4 权限依赖 

**2.4.1 添加接口权限**

在您项目中的 `manifest.json` 文件中的 `features` 属性中添加权限声明代码。如下：

```javascript
    // manifest.json 文件
    
    "features": [
        { "name": "system.fetch"},
        {"name": "system.storage"},
        {"name": "system.device"},
        {"name": "system.network"},
        {"name": "service.account"},
        {"name": "system.shortcut"}
    ]

```

#### 2.5 引入 SDK 代码

在项目的 `app.ux` 文件的 `script` 标签中引入统计代码：

```javascript
    // app.ux 文件
    
    import "./appStatistics.min.js"


``` 

#### 2.6 添加打点代码


**2.6.1 初始化打点**

在 `app.ux` 的 `onCreate` 函数顶部（其他代码之前），增加统计打点代码：

```javascript

 // app.ux 文件
<script>

    import "./appStatistics.min.js"

    export default {

        onCreate:function(){
            
            //统计打点
            APP_STATISTICS.app_sta_init( this );

            // 其他业务代码...

        }
    }
        
</script>

```


**2.6.2 页面打点**

在快应用的所有页面文件（ `manifest.json 中 router > pages` 对象下声明的页面对应的`.ux`文件中 ）中增加统计代码，有两种接入方法：

- **方法一**：在 App 中每个路由页面的`onShow`和`onHide`方法中调用`APP_STATISTICS.page_show()`和`APP_STATISTICS.page_hide()`方法，添加如下代码:

```javascript

// 页面文件 xx.ux

<template>
    <div><text>这是首页</text></div>
</template>

<script>

    export default {

        onShow(){
            APP_STATISTICS.page_show( this );//在onShow方法的第一行加入此代码
            //App业务代码
        },
        onHide() {
            APP_STATISTICS.page_hide( this ); //在onHide方法的第一行加入此代码
            //...业务代码
        } 
        data: {
            a:1,
            b:[]
        },
        onInit () {
            //...业务代码
        },
        onReady(){
            //业务代码...
        }        
    }
        
</script> 
    
```

- **方法二**：这种方式更加简洁，开发者无需自己调用 `APP_STATISTICS.page_show()` 和`APP_STATISTICS.page_hide()` 方法，而是通过 `sdk` 提供的的全局函数 `Custom_page()` 实页面计埋点。参考代码如下:
  

```javascript

// 页面文件 xx.ux

<template>
    <div><text>这是首页</text></div>
</template>

<script>
  
    export default Custom_page ({
        data: {
            a:1,
            b:[]
        },
        onInit () {
            //业务代码...
        },
        onReady(){
            //业务代码...
        }
        //业务代码...
    })
</script> 

```  

#### 2.7 接入完成

确认完成以上步骤之后，就可以使用我们提供的基础统计统计功能啦。关于如何检查统计 `sdk` 接入是否成功请移步 “FAQ常见问题”。


### 3. 注意事项

 1. 一个 `app_key` 只能对应一个快应用，请不要重复使用，否则会造成数据不准确；
 2. 确保在路由中配置的所有页面都增加统计代码，否则会造成数据不准确、统计数据缺失；
 3. 请开发者慎重调用 `storage.clear(OBJECT)` 接口！统计`SDK`会将用户相关操作数据缓存在客户端数据存储模块。若调用该接口可导致数据统计不准确的问题。


[1]: http://www.qinglizi.cn/account/login

[2]: http://www.qinglizi.cn/quickapp_statistics_sdk.zip
