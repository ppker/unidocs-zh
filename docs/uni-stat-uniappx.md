# uni-app x VDOM 模式统计接入

本页说明 uni-app x App VDOM 模式下的统计接入方式。

uni-app x App 蒸汽模式、Web 和微信小程序从 `HBuilderX 5.25` 起使用框架内置的 uni统计2.0，配置和使用方式与公有版、私有版文档一致，请分别参见 [uni统计2.0 公有版](uni-stat-public.md) 或 [uni统计2.0 私有版](uni-stat-private.md)。

## VDOM 模式@vdom

本节适用于 uni-app x App VDOM 模式。当前 Android、iOS VDOM 模式不能使用 HBuilderX 5.25 新增的框架内置统计接入方式，仍需使用独立的 `uni_modules/uni-stat` 插件，并手工接入应用级生命周期。

::: warning 注意

本页方案不适用于 uni-app x App 蒸汽模式。蒸汽模式项目请使用 HBuilderX 内置的 uni统计2.0，不要同时引用 `uni_modules/uni-stat`，否则可能造成重复采集和重复上报。

:::

### 配置 uni统计后台

VDOM 插件方案的后台配置方式与 uni统计2.0 私有版相同，但两者的数据不互通。后台部署参见 [创建 admin 项目](uni-stat-private.md#创建-admin-项目)。

### 前端配置

VDOM 模式使用独立插件，需要在插件市场下载 [uni统计](https://ext.dcloud.net.cn/plugin?name=uni-stat)。无需在 `manifest.json` 中配置内置统计开关，插件参数在 `main.uts` 中传入。

#### uniStatistics 配置

|字段|类型|默认值|说明|
|:-:|:-:|:-:|:-:|
|debug|Boolean|false|开启统计调试模式，会产生较多日志；正式发布请关闭|
|reportInterval|Number|10|前端数据上报周期，单位为秒|
|collectItems|Object|-|采集项配置|

`collectItems` 配置：

|字段|类型|默认值|说明|
|:-:|:-:|:-:|:-:|
|uniPushClientID|Boolean|false|是否采集 PushClientID。App 端需要先初始化 uni-push SDK，并按要求完成隐私合规配置|
|uniStatPageLog|Boolean|true|是否采集页面数据；不需要页面统计时可以关闭|

#### 引用插件

在 `main.uts` 中以 Vue 插件方式加载：

```js
import App from './App.uvue'
import { createSSRApp } from 'vue'
import { uniStat } from '@/uni_modules/uni-stat/plugin.uts'

const uniStatOptions = {
  debug: true,
  collectItems: {
    uniStatPageLog: true
  }
}

export function createApp() {
  const app = createSSRApp(App)

  // use 的第二个参数不能为空，使用默认配置时传入 {}
  app.use(uniStat, uniStatOptions)

  return {
    app
  }
}
```

### 应用生命周期上报

VDOM 模式下，统计插件无法自动获取应用启动、前后台切换和全局错误等应用级生命周期，需要在 `App.uvue` 中手工调用 `uni.report()`。

```js
<script lang="uts">
export default {
  onLaunch: function (options) {
    uni.report({
      name: 'uni-app-launch',
      options: options,
      success(res) {
        console.log(res)
      },
      fail(err) {
        console.log(err)
      }
    })
  },
  onShow: function () {
    uni.report({
      name: 'uni-app-show',
      success(res) {
        console.log(res)
      },
      fail(err) {
        console.log(err)
      }
    })
  },
  onHide: function () {
    uni.report({
      name: 'uni-app-hide',
      success(res) {
        console.log(res)
      },
      fail(err) {
        console.log(err)
      }
    })
  },
  onError: function (error) {
    uni.report({
      name: 'uni-app-error',
      options: error,
      success(res) {
        console.log(res)
      },
      fail(err) {
        console.log(err)
      }
    })
  }
}
</script>
```

### 生命周期对统计的影响

- `uni-app-launch`：应用启动事件，影响日活、新增和总设备数。
- `uni-app-show`：应用从后台进入前台，影响日活。
- `uni-app-hide`：应用从前台进入后台，影响页面访问时长和相关页面统计。

### uni.report(OBJECT)

`uni.report` 的参数和回调请参考 [uni-app x report API](https://doc.dcloud.net.cn/uni-app-x/api/report)。
