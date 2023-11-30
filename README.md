---
title: 前端监控与探索埋点方案
---

# 前端监控与探索埋点方案

## 前言

优秀的产品有吸引之处，糟糕的产品则有许多问题。优秀产品通过反馈和用户体验的改进而不断进化，因此了解用户在产品上的活动至关重要。前端监控通过数据收集和统计用户行为来实现这一目标，接下来我们将深入探讨前端监控和埋点技术。

## 前端监控

### 什么是前端监控？

前端监控是一种技术和方法的综合应用，用于追踪和记录在线产品或服务的用户界面交互、性能指标和异常情况。从B端的角度，前端监控是企业用于了解其在线产品的运行状况、用户满意度和性能的重要工具。以下是前端监控的核心组成部分：

**1、数据监控**

数据监控，顾名思义就是监听用户的行为，如：

- 页面浏览量或点击量（PV）和终端访问数量（UV）：了解产品的流量情况。
- 用户在每个页面停留时间：分析用户对不同页面的交互情况。
- 用户访问入口来源：了解访问途径、使用的搜索引擎等。
- 用户在页面上的操作：监测用户的点击、滚动和其他交互行为。

**2、性能监控**

这些监控结果显示前端性能，指导性能优化，提高用户体验，如：

- 不同用户、设备和系统的页面加载时间：确保页面在各种情况下都能快速加载。
- 白屏时间：衡量用户看到页面内容之前的等待时间。
- HTTP请求响应时间：监测网络请求的效率和速度。
- 静态资源下载时间：确保CSS、JavaScript等静态资源的快速加载。
- 页面渲染和交互时间：衡量用户与页面互动的响应速度。

**3、异常监控**

异常监控帮助捕获难以预测的异常，确保产品稳定性，如：

- Javascript的异常监控。
- 样式丢失的异常监控。

### 为什么需要前端监控？

前端监控对企业来说至关重要，因为它可以提供多方面的商业价值：

- 优化用户体验：通过监控用户行为和性能，企业可以快速识别和解决与用户体验相关的问题，提高客户满意度。
- 增加收入和业务增长：降低页面加载时间，减少错误，从而提高收入和业务增长。
- 降低成本：及时捕获和解决前端错误可以减少客户支持成本，降低维护成本。
- 数据驱动决策：根据用户行为和产品性能等数据，帮助企业优化产品和服务以满足客户需求。

用一句话总结就是：**获取用户行为以及跟踪产品在用户端的使用情况，并以监控数据为基础，指明产品优化的方向。**

### 如何实施前端监控？

实现前端监控有三个步骤：前端埋点和上报、数据处理和数据分析。

#### 第一步：前端埋点和上报

前端埋点指代在应用程序或网站中插入代码或标记，以便收集特定事件、行为或数据。这些事件和数据的采集可以用于数据分析、监控、性能优化和用户体验改进。

目前方式主流的埋点方式：代码埋点、可视化埋点、无埋点。

**1、代码埋点**

通过代码主动将所需信息上报给服务器。可以采集到丰富的用户行为数据，精准定义功能事件灵活性高，可控制数据获取的时机和方式。但工作量大，人力成本高，需要专人负责，发现错漏无法快速事后补救，跨版本管理成本高，废点会造成代码垃圾也会影响性能。

- 优点：精确发送所需的数据，灵活性高。
- 缺点：代码分散，难以维护，需要开发人员手动添加埋点。

**2、可视化埋点**

用可视化工具代替手动埋点，将业务代码和埋点分开。通过可视化工具配置埋点，需要另外配套一个平台控制埋点的埋入，优点是自动生成埋点代码嵌入到页面中，减轻业务开发人员的埋点负担，但无法做到自定义获取数据，仅支持前端界面行为分析(如点击次数)，只支持对可见元素采集信息。

- 优点：允许非技术人员选择埋点区域，有圈选系统。
- 缺点：埋点控件有限，无法完全手动定制，不适用内网系统。

**3、无埋点**

也叫全(自动)埋点，全局监听系统事件，记录用户所有行为。无埋点的数据获取全面，不会出现漏埋、误埋等现象无需开发，先报数据后埋点。但数据只能采集点击、展示等简单用户行为，无法掌握用户身份信息和行为信息等，采集的数据量大，对用户产品使用的消耗增大并且数据需要二次梳理加工，后期数据加工压力大。大部分只适用于pv/uv等较简单的指标。

- 优点：记录全面，无需手动添加埋点代码。
- 缺点：增加数据传输和服务器压力，无法灵活定制事件数据。

选择哪种方式的选择应该基于具体业务场景，不同的业务分析场景，对于数据的完备性和多维度的要求是完全不同的。下面是它们相比的优缺点的总结：

| 比较项     | 代码埋点 | 可视化埋点/全埋点 |
| ---------- | -------- | ----------------- |
| 准确性     | 高       | 较高              |
| 兼容性     | 好       | 中等              |
| 稳定性     | 稳定     | 不稳定            |
| 自定义属性 | 支持     | 不支持            |
| 管理成本   | 中等     | 较高              |
| 埋点成本   | 较高     | 无                |
| 回溯历史   | 不支持   | 支持              |

可以看出，在埋点本身质量上，代码埋点是要远优于可视化全埋点的，但是可视化全埋点在埋点成本和回溯历史方面有着无可比拟的优势。从建设更坚实的数据根基的角度来说，首推代码埋点，但是如果面向一些短平快的分析诉求，可以考虑使用可视化全埋点。

#### 第二步：数据处理

包括清理、筛选、格式化和合并收集到的数据，以确保数据的准确性和一致性，准备数据以供分析。

#### 第三步：数据分析

涉及定义关键性能指标、数据可视化、趋势分析、关联分析和异常检测，以从数据中提取有价值的见解，支持业务决策和行动计划。

## 探索埋点方案

根据项目的复杂度，可以按需设计出符合自身的埋点，下面我将从埋点的考虑与原则、确定埋点范围与编码、确定上报方式以及数据格式来逐步设计符合自己的埋点方案。

### 考虑与原则

- 方便团队合作，尽可能减少团队成员的学习成本和编写代码。
- 不影响前后端原本的性能。
- 需考虑后期扩展，支持提供开启关闭埋点功能。
- 清晰统一的上报数据结构便于后期做分析和应用。
- 考虑现状，与后端同事紧密结合讨论设计。

### 埋点流程

![image.png](https://www.itcast.cn/files/image/202302/20230216150451872.png)

### 埋点采集数据类别

- 错误采集
  - 资源加载错误,代码异常(error)
  - promise调用链异常(reject)
  - console.error异常
- 请求采集
  - xhr & axios & fetch
- 访问（页面）采集
  - history.pushState history.replaceState
- 资源采集
  - DOM加载、资源加载
- 事件采集
  - 点击事件、自定义事件

### 上报数据格式

#### 设想格式

所有上报的数据格式应该包含两类，一类是基础信息，比如：用户标识信息（uid）、上报类型（type），上报时间（startTime），上报地址（path）等信息，一类是根据不同上报类型而制定相应的信息，大致上报格式为：

```js
{
  uid: "3892337188223",
  type: "CODE_ERR",
  startTime: "1697683330712",
  path: "http://xxx",
  ...
}
```

PV/UX关注的数据

```js
{
  fromPath: "来源路由",
  toPath: "目标路由",
  inTime: "页面进入时间",
  outTime: "页面离开时间",
  ...
}
```

点击事件/输入事件关注的数据

```js
{
  eleId: "元素id",
  attrs: "元素所有属性",
  val: "元素文本",
  ...
}
```

性能关注的数据

```js
{
  WT: "白屏时间",
  ONL: "执行onload事件耗时",
  TTFB: "读取页面第一个字节的时间",
  ...
}
```

异常关注的数据

```js
{
  info: "错误信息",
  msg: "异常摘要",
  ...
}
```

#### 基于现状制定格式

公共数据采集

| 属性名     | 说明                          | 示例                                                          |
| ---------- | ----------------------------- | ------------------------------------------------------------- |
| id         | ID（自动生成）                | "652f467f5d2816041badfff1"                                    |
| type       | 类型（参考：SEND_TYPE）       | "error"                                                       |
| trigger    | 触发源（参考：SEND_TRIGGER）  | "server"                                                      |
| createTime | 创建时间                      | 1698132307306                                                 |
| userId     | 用户 ID                       | "3426309289734957438575599677321"                            |
| version    | 版本号                        | "1.0.0"                                            |
| ip         | IP 地址                       | "127.0.0.1"                                                   |
| referer    | Referer                       | "https://www.baidu.com" |
| userAgent  | User-Agent                    | "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7)"             |
| vp         | 网页可见区大小                | "1600 \* 1240"                                                |
| sr         | 显示屏幕大小                  | "2560 \* 1440"                                                |
| sdkVersion | SDK版本号                     | "0.0.1"                                                       |
| vendor     | 浏览器名称                    | "Google Inc."                                                 |
| platform   | 浏览器平台的环境              | "MacIntel"                                                    |
| sessionId  | 会话 ID                       | "s_134b3d97-5fc2d010-6b156f574193e7fe"                        |
| deviceId   | 设备 ID                       | "t_134b3d97-5fc4dc33-0879cdfac0d43270"                        |

```js
// 类型
export enum SEND_TYPE {
  PV = 'pv',                     // 路由
  ERROR = 'error',               // 错误
  PERFORMANCE = 'performance',   // 资源
  CLICK = 'click',               // 点击
  DWELL = 'dwell',               // 页面卸载
  CUSTOM = 'custom',             // 手动触发事件
  INTERSECTION = 'intersection'  // 曝光采集
}

// 触发源
export enum SEND_TRIGGER {
  PAGE = 'page',                 // 页面
  RESOURCE = 'resource',         // 资源
  SERVER = 'server',             // 请求
  CODE = 'code',                 // code
  REJECT = 'reject',             // reject
  CONSOLEERROR = 'console.error' // console.error

  // target.nodeName ( 元素名 ps:script, 在html元素上发生异常时会作为eventId )
  // pageId ( 在页面跳转时会带上应用ID作为eventId )
}
```

异常采集

| 属性名称   | 值  | 说明                           |
| ---------- | --- | ------------------------------ |
| errMessage |     | 错误信息                       |
| errStack   |     | 完整的错误信息                 |
| line       |     | 错误信息发生在第几行           |
| col        |     | 错误信息发生在第几列           |
| params     |     | 主动方法触发错误收集可以带参数 |

```js
// {type: 'error',trigger: 'code' }
{
  errMessage: 'a.split is not a function',
  line: '288',
  col: '9'
}
```

请求采集

| 属性名称       | 值                   | 说明         |
| -------------- | -------------------- | ------------ |
| requestUrl     |                      | 请求地址     |
| requestMethod  | get、post...         | 请求方式     |
| requestType    | xhr、fetch...        | 请求类型     |
| responseStatus |                      | 请求返回代码 |
| duration       | 请求正确时才有此字段 | 请求消耗时间 |
| params         |                      | 请求的参数   |
| errMessage     | 请求错误时才有此字段 | 请求错误信息 |

```js
// 请求正确时 {type: 'performance',trigger: 'server' }
{
  requestUrl: 'http://localhost:6656/getList?test=123',
  requestMethod: 'get',
  requestType: 'xhr',
  responseStatus: 200,
  duration: 13,
  params: { test: '123' },
}

// 请求错误时 {type: 'error',trigger: 'server' }
{
  requestUrl: 'http://localhost:6656/getList2?test=123',
  requestMethod: 'get',
  requestType: 'xhr',
  responseStatus: 404,
  params: { test: '123' },
  errMessage: 'Not Found',
}
```

访问采集

| 属性名称 | 值                                          | 说明         |
| -------- | ------------------------------------------- | ------------ |
| referer  |                                             | 上级页面URL  |
| title    | document.title                              | 页面标题     |
| action   | navigate / reload / back_forward / reserved | 页面加载来源 |

```js
// {type: 'pv',trigger: '134b23f7-56a67609-802eb5fc1a34fde9' }
{
  referer: 'http://localhost:6656/#/err',
  title: 'example-vue2',
  action: 'navigation',
}
```

资源采集

分为首屏加载和资源加载

首屏加载

| 属性名称   | 值                                            | 说明                 |
| ---------- | --------------------------------------------- | -------------------- |
| appcache   | t.domainLookupStart - t.fetchStart            | dns缓存时间          |
| dom        | t.domInteractive - t.responseEnd              | dom解析耗时          |
| dns        | t.domainLookupEnd - t.domainLookupStart       | dns查询耗时          |
| firstbyte  | t.responseStart - t.domainLookupStart         | 首包时间             |
| fmp        | t.fetchStart                                  | 首屏时间             |
| loadon     | t.loadEventStart - t.fetchStart               | 页面完全加载时间     |
| ready      | t.domContentLoadedEventEnd - t.fetchStart     | HTML加载完成时间     |
| res        | t.loadEventStart - t.domContentLoadedEventEnd | 同步资源加载耗时     |
| ssllink    | t.connectEnd - t.secureConnectionStart        | SSL安全连接耗时      |
| tcp        | t.connectEnd - t.connectStart                 | tcp连接耗时          |
| trans      | t.responseEnd - t.responseStart               | 内容传输耗时         |
| ttfb       | t.responseStart - t.requestStart              | 请求响应耗时         |
| tti        | t.domInteractive - t.fetchStart               | 首次可交互时间       |
| redirect   | t.redirectEnd - t.redirectStart               | 重定向时间           |
| unloadTime | t.unloadEventEnd - t.unloadEventStart         | 上一个页面的卸载耗时 |

```js
// {type: 'performance',trigger: 'page' }
{
  fmp: 261.7,
  tti: 103.8,
  ready: 230.6,
  loadon: 304.7,
  firstbyte: 10,
  appcache: 3.3,
  tcp: 0.3,
  ttfb: 9.7,
  trans: 1.5,
  dom: 89,
  res: 74.1,
  ssllink: 5.1,
  unloadTime: undefined,
  redirect: undefined,
  dns: undefined,
}
```

资源加载

| 属性名称          | 值  | 说明                                   |
| ----------------- | --- | -------------------------------------- |
| requestUrl        |     | 资源具体url                            |
| initiatorType     |     | 通过某种方式请求的资源,比如script,link |
| transferSize      |     | 传输的数据包大小                       |
| encodedBodySize   |     | 数据包压缩后大小                       |
| decodedBodySize   |     | 数据包解压后大小                       |
| duration          |     | 加载具体时长                           |
| redirectStart     |     | 重定向开始时间                         |
| redirectEnd       |     | 重定向结束时间                         |
| startTime         |     | 开始时间                               |
| fetchStart        |     | 开始发起请求时间                       |
| domainLookupStart |     | DNS开始解析时间                        |
| domainLookupEnd   |     | DNS结束解析时间                        |
| connectStart      |     | 开始建立连接时间                       |
| connectEnd        |     | 连接建立完成时间                       |
| requestStart      |     | 开始发送数据包时间                     |
| responseStart     |     | 开始接收数据包时间                     |
| responseEnd       |     | 数据包接收完成时间                     |

```js
// {type: 'pv',trigger: '134b23f7-56a67609-802eb5fc1a34fde9' }
> 加载错误的资源会产生两个事件【1.资源本身的加载情况 2.报错情况】

// 资源本身的加载情况
// {type: 'performance',trigger: 'resource' }
{
  initiatorType: 'script',
  duration: 1239,
  startTime: 271471.5,
  fetchStart: 271471.5,
  responseEnd: 272710.5,
  requestUrl: 'https://cdn.jsdelivr.net/npm/lodash22',
}

// 报错情况
// {type: 'error',trigger: 'resource' }
{
  initiatorType: 'script',
  requestUrl: 'https://cdn.jsdelivr.net/npm/lodash22',
}
```

事件采集

| 属性名称    | 值                   | 说明                     |
| ----------- | -------------------- | ------------------------ |
| title       | 详见下面的采集规则   | 事件名                   |
| params      | 详见下面的采集规则   | 事件参数                 |
| elementPath | 例如: div>div>button | 被点击元素的层级         |
| x           | 见下方               | 被点击元素与屏幕左边距离 |
| y           | 见下方               | 被点击元素与屏幕上边距离 |

```js
// {type: 'click',trigger: 'div' }
{
  title: 'xxx',
  x: 280,
  y: 20,
  params: { bigtitle: 'bigTitle' },
  elementPath: 'div>div>div>div',
}
```

### 上报方式

**1、AJAX请求**

最普通的前端发送一个做好的接口请求。

```js
http.post("/logger", { key: "001", name: "发送信息" });
```

- 优点：灵活、异步、可控制。
- 缺点：受同源策略限制，性能开销较大。

**2、`<img>` 标签**

利用图片的 src 属性。

```js
function send(baseURL, query = {}) {
  let queryStr = Object.entries(query)
    .map(([key, value]) => `${key}=${value}`)
    .join("&");
  let img = new Image();
  img.src = `${baseURL}?${queryStr}`;
}

send("/image.gif", { key: "001", name: "发送信息" });
```

- 优点：跨域支持，简单易用，占用存储空间小。
- 缺点：只支持GET请求，数据传输有限。

**3、Web Beacon**

```js
Navigator.sendBeacon(url, { key: "001", name: "发送信息" });
```

- 优点：低性能开销，异步，不阻塞页面刷新和跳转。
- 缺点：可能不兼容旧浏览器。

总之，选择哪种数据上报方式应该根据具体需求和约束来决定。AJAX请求提供更多的控制，但需要处理跨域和可能引入性能开销。通常 `<img>` 标签出现的需要跨域的场景中。而 `navigator.sendBeacon` 可以避免在页面关闭时阻塞。当然如果使用不了（例如IE浏览器）可以用 `<img>`。

### 技术实现

分为自动上报和手动上报，而且 80% 以上应该实现自动上报，比如异常类的，性能类，部分事件的都可以全局统一去埋点，而少部分或定制化的需求才会考虑手动上报，比如点击页签、点击某个按钮等。

手动埋点的话，大致是页面交互埋点使用Vue自定义指令(声明式埋点)完成，PV 埋点使用路由守卫完成。

交互埋点思路（以Vue2为例）

```js
/* >>> 命令式埋点 <<< */
Vue.prototype.$logger = logger;
this.$logger.track({ key: "download", name: "下载" });

window.logger.track({ key: "download", name: "下载" });


/* >>> 声明式埋点 <<< */
Vue.directive('track', {});
<div v-track:click></div>
<div v-track="{ key: 'download', name: '下载' }"></div>
```

pv 埋点思路

```js
trackPage(router);
```

## 未来期望

- 实时监控事件，也可设定时间区间按需监控。
- 诊断分析，比如链路追踪，错误分析，日志分析等。

## 关键词

Frontend Monitoring、Event tracking、Data Tracking、User Analytics、Event Logging。
