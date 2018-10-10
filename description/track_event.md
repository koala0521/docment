# 自定义事件

---

### 1. 定义和用途

自定义事件，用于记录用户操作行为的具体细节，具体的，开发者可通过在代码中埋点的方式统计自己期望了解的快应用数据，如：用户点击某功能按钮、填写某个输入框、触发了某个广告等，甚至可以用来统计一段特定的代码是否被执行。

### 2. 参数说明

#### 2.1事件名称
可由开发者自行定义，为事件 ID 的一个中文翻译名称，是为了方便运营人员查看；
事件名称命名规则：
事件名称可以使用数字、中文、字母及下划线的组合，且长度不能超过 255 字符。

#### 2.2事件ID及相应参数

可由开发者自行定义，将该事件 ID 及相应参数写入需要跟踪的位置中即可；
事件 ID 命名规则：
事件 ID 名称可以使用数字、中文、字母及下划线的组合，且长度不能超过 255 字符；

事件参数命名规则：
（具体对应参数 value ，该参数可以为一个字符串( String )或者一个JavaScript对象 ( Object )；
规则如下：
1. value 为 string 类型时，字符长度必须小于 255
2. value 为 object 类型时，该对象的值只能为 string 类型
3. 字符串可以使用数字、中文、字母及下划线的组合	
满足上述规则时，SDK 才会上报事件及其参数。否则 SDK 不会上报。）

注意：每个快应用下最多支持创建500个自定义事件，每个自定义事件至多支持创建100个参数，每个参数至多支持创建1000个不同取值；否则数据将不会上报或不计算。

#### 2.3触发人数

即触发该事件的人数；根据用户唯一标识对事件发生次数进行去重。

#### 2.4触发次数

即该事件总共发生的次数。