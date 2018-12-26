# 错误分析接入指南

---




#### 1. 定义和用途

错误分析，支持对您快应用的日常错误信息进行收集、归类，以便帮助您快速定位错误，进而针对性修复优化产品设计给用户带来更好体验。

#### 2. 埋点方式

```javascript

// app.ux 文件

<script>

import util from './util'
import './appStatistics.min';

export default {

    showMenu: util.showMenu,
    createShortcut: util.createShortcut,
    onCreate: function () {

        APP_STATISTICS.app_sta_init(this);
    },

	//  监听应用错误的生命周期函数
    onError(err) {
        try {
			// 错误统计打点代码
            APP_STATISTICS.on_err(err);
        } catch (error) {}

    }
}
</script>


```
#### 3. 注意事项

1. 由于错误分析相关功能依赖“基础统计”接口提供，因此接入错误分析之前请确保已成功接入“基础统计”（基础统计接入方法具体请参见“轻粒子快应用统计平台 SDK 接入指南”）；小轻助手-错误分析助您快速定位错误，为用户带来更好体验；

2. 错误分析是基于快应用平台1030版本新增接口实现的；因此，平台版本低于1030（以及未接入轻粒子最新版本SDK1.3.2.0）快应用发生错误时，将无法成功统计到。

3. 友情提示：请尽快升级轻粒子最新版本SDK1.3.2.0，同时想更快了解快应用版本、SDK版本、平台版本相关情况，请参见轻粒子快应用统计平台-用户行为分析-版本分析！