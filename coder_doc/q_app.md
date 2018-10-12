# 快统计 SDK 接入指南

---

### 1.申请app_key

注册账号并登录 [快统计官网][1] 在“快应用中心”页面点击创建快应用，成功后就可以获取到快应用app_key。


### 2.接入 SDK

#### 2.1 下载SDK
下载 SDK文件并解压，将其中的 appStatistics.min.js、statistics.config.js拷贝到app.ux同级的目录（src目录下）。
下载地址： [SDK下载](https://kss.ksyun.com/file-mha/quickapp_statistics/sdk/quickapp_statistics_sdk.zip​)


#### 2.2 SDK导入项目
将 sdk 中的 appStatistics.min.js、statistics.config.js拷贝到app.ux同级的目录（src目录下）。


#### 2.3 配置app_key

用编辑器打开statistics.config.js ，填入从官网获取的 app_key ：


#### 2.4 安装依赖


**2.4.1 安装项目依赖**

```json
 npm install --save-dev babel-preset-stage-3
 npm install --save md5

```


**2.4.2 配置预设： .babelrc 文件中，增加预设 "stage-3"** 

```javascript
 // .babelrc文件

    {
        "presets": [
            "env",
            "stage-3"
        ]
    }

```

**注意**：如果接入过程中发现项目中没有 .babelrc 文件，请在项目根目录手动创建即可。

**2.4.3 给 SDK 使用的接口添加权限**

在您项目中的manifest.json文件中的 features 属性中添加代码如下：

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

在项目的 app.ux 文件的 script 标签中引入统计代码：

```javascript
    // app.ux 文件
    
    import "./appStatistics.min.js"


``` 

#### 2.6 添加打点代码


**2.6.1 初始化打点**

在 app.ux 的 onCreate 函数顶部（其他代码之前），增加统计打点代码：

```javascript
    // app.ux 文件

    onCreate:function(){
        
        //统计打点
        APP_STATISTICS.app_sta_init( this );

        // 其他业务代码...

    },        

```


**2.6.2 页面打点**

在快应用的所有页面文件（ manifest.json 中 router > pages 对象下声明的页面对应的组件中 ）中增加统计代码，有两种接入方法：

- **方法一**：在App中每个路由页面的onShow和onHide方法中调用APP_STATISTICS.page_show()和APP_STATISTICS.page_hide()方法，添加如下代码:

```javascript

<script>
     
    onShow(){
        APP_STATISTICS.page_show( this );//在onShow方法的第一行加入此代码
        //App业务代码
    },
    onHide() {
        APP_STATISTICS.page_hide( this ); //在onHide方法的第一行加入此代码
        //...业务代码
    } 
    data: {
        //...业务代码
    },
    onInit () {
        //...业务代码
    },

</script> 
    
```

- **方法二**：开发者无需自己调用 APP_STATISTICS.page_show() 和APP_STATISTICS.page_hide() 方法，而是通过 sdk 提供的的全局函数 Custom_page() 实页面计埋点。
  

```javascript

<script>
  
    export default Custom_page ({
        data: {
            a:1,
            b:[ 1,2 ]
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

确认完成以上步骤之后，就可以使用我们提供的基础统计统计功能啦。

<!-- ### 3. 高级功能

#### 3.1 自定义事件

#### 3.1.1 定义和用途

自定义事件，用于记录用户操作行为的具体细节，具体的，开发者可通过在代码中埋点的方式统计自己期望了解的快应用数据，如：用户点击某功能按钮、填写某个输入框、触发了某个广告等，甚至可以用来统计一段特定的代码是否被执行。通过事件分析，可以对用户在快应用内的行为做精细化跟踪，满足页面访问等标准统计以外的个性化分析需求。

#### 3.1.2 打点接口及埋点说明

#### 3.2 接口说明

打点接口：

```javascript

    APP_STATISTICS.track_event(id,attr);

```

- 描述：

  该方法接收两个参数，第一个参数key为事件名称（必传）。第二个参数 vaule 为事件本身的参数（非必传）。该参数可以为一个字符串( String )或者一个 JavaScript 对象 ( Object )。

- id：

  事件的唯一标识符，不可重复。登录 [快统计官网][1] 点击[ 我的数据 ] > [ 高级功能 ] > [ 自定义事件 ] > [ 创建自定义事件 ] 创建事件id。


- 参数：

    ```javascript

    key: string  
    value: {  string | object }

    ```

规则：

1. key 为 string 类型，并且字符长度必须小于 255

2. value 为 string 类型时，字符长度必须小于 255

3. value 为 object 类型时，该对象的值只能为 string 类型

4. 字符串支持数字、中文、字母及下划线的组合

满足上述规则时，SDK 才会上报事件及其参数。否则 SDK 不会上报。


#### 2.2 埋点方式


仅统计事件，无参数时，可使用如下方法：

```javascript

    APP_STATISTICS.track_event('事件id');

```

统计事件携带参数，且参数为字符串：

```javascript

    APP_STATISTICS.track_event('事件id' , '参数' );

```

统计事件携带参数，且参数为 object ：

```javascript

    APP_STATISTICS.track_event('事件id' , {
        参数_1: '参数值',
        参数_2: '参数值'        
    });

``` -->

### 3. 注意事项

 1. 一个 app_key 只能对应一个快应用，请不要重复使用，否则会造成数据不准确；
 2. 确保在路由中配置的所有页面都增加统计代码，否则会造成数据不准确、统计数据缺失；
 3. 请开发者慎重调用 storage.clear(OBJECT) 接口！统计SDK 会将用户相关操作数据缓存在客户端数据存储模块。若调用该接口可导致数据统计不准确的问题。


[1]: http://ktj.wankacn.com/login

[2]: https://kss.ksyun.com/file-mha/quickapp_statistics/sdk/quickapp_statistics_sdk.zip