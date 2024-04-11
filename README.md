# xyn-circle-progress

介绍：方便在 uniapp 中使用环形进度条，且在 h5 和微信小程序中使用正常，使用 css 完成进度条样式，防止 canvas 在微信小程序中层级问题。
关键词：uniapp circle-progress vue3 环形进度条 圆形进度条 css

1. 安装
2. 使用
3. 配置
4. API

# 1. 安装

安装使用 npm，`npm install xyn-circle-progress`

# 2.使用

2.1 引入 xyn-circle-progress 组件

`import xynCircleProgress from "xyn-circle-progress";`

2.2 页面中写入组件

`<xyn-circle-progress :rate="80" boxWidth="160rpx"  width="15rpx" activeColor="#99C39C" inactiveColor="#F2F2F2" :fillet="true" >  
 <view>{{80}}%</view>  
 </xyn-circle-progress>  
 `

# 3.API

|     字段      |  类型   | 必填 | 默认值  |       描述       |
| :-----------: | :-----: | :--: | :-----: | :--------------: |
|     rate      | 数据源  |  是  |   无    |    进度 0-100    |
|   boxWidth    | String  |  否  | 200rpx  | 进度条图形的宽度 |
|     width     | String  |  否  |  20rpx  | 进度条的线条宽度 |
|  activeColor  | String  |  否  | #54c4fd |    进度条颜色    |
| inactiveColor | String  |  否  | #546063 |  进度条背景颜色  |
|  startAngle   | String  |  否  |    0    |     开始角度     |
|  startAngle   | Boolean |  否  | String  |   是否使用圆边   |
