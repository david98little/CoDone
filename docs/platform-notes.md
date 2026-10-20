# 微信平台能力与限制

CoDone 的产品设计必须建立在微信小程序现有能力边界上。本文记录与 MVP 直接相关的平台规则和官方文档，供产品、设计和开发共同核对。

## 1. 小程序订阅消息

- 订阅消息出现在微信“服务通知”里，点击消息卡片可以跳转到小程序页面。
- 一次性订阅：用户每完成一次订阅，开发者可以不限时间地下发一条对应模板消息；每条消息需要单独订阅或退订。
- 长期订阅：仅向政务民生、医疗、交通、金融、教育等线下公共服务开放；CoDone 的“工具-效率”类目不适用。
- 用户必须在小程序内通过点击行为触发订阅；不能静默订阅，也不能替好友订阅。
- 用户勾选“总是保持以上选择，不再询问”后，后续调用订阅接口不会弹窗，但不改变“一次订阅一条”的性质。
- 订阅消息可能受到用户免打扰、微信通知关闭、系统限制等影响，不能承诺强提醒。

官方文档：

- [小程序订阅消息概述](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/subscribe-message-overview.html)
- [小程序订阅消息开发指南](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/subscribe-message.html)
- [wx.requestSubscribeMessage](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/subscribe-message/wx.requestSubscribeMessage.html)

## 2. 分享与好友协同

- 小程序可以通过 `onShareAppMessage` 分享给微信好友或群聊。
- 平台不提供读取好友列表、自动加好友、代发好友消息的接口。
- 邀请必须由用户主动分享卡片完成；好友打开卡片后进入小程序并加入清单。
- `shareTicket` 只在特定群聊场景下有效，不能作为长期群身份使用。

官方文档：

- [小程序转发](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/share.html)
- [wx.showShareMenu](https://developers.weixin.qq.com/miniprogram/dev/api/share/wx.showShareMenu.html)

## 3. 聊天工具模式

- 聊天工具模式用于群任务、群接龙、群问卷等场景，可绑定单聊或群聊。
- 基础库 3.7.8 起支持；微信 8.0.56 及以上版本。
- 聊天工具分包需使用 Skyline 渲染，分包体积不超过 500KB。
- 每个小程序目前只能配置一个聊天工具，需要审核。
- 聊天工具模式内普通转发、外跳小程序、广告等能力受限。
- `wx.notifyGroupMembers` 用于提醒群成员完成任务，但只能在用户点击行为时调用，不能定时自动发。

官方文档：

- [小程序聊天工具模式](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/chatTool.html)
- [wx.notifyGroupMembers](https://developers.weixin.qq.com/miniprogram/dev/api/chattool/wx.notifyGroupMembers.html)

## 4. 小程序类目与主体

- 微信小程序类目表里，个人主体可以选择“工具-效率”，适用于日常生活、工作效率提升类服务。
- 具体类目和资质以提交时微信公众平台要求为准。
- 聊天工具模式是 Beta 能力，个人主体能否稳定通过审核需要实际提测验证。

官方文档：

- [小程序开放的服务类目](https://developers.weixin.qq.com/miniprogram/product/material/)

## 5. 服务号模板消息（不在 MVP 内）

- 服务号模板消息要求认证服务号、用户关注和对应行业类目，且模板内容受服务场景限制。
- 它不是绕过小程序订阅消息规则的手段。
- CoDone MVP 不依赖服务号模板消息；后续如需提高到达率，再单独评估。

官方文档：

- [服务号模板消息](https://developers.weixin.qq.com/doc/offiaccount/Message_Management/Template_Message_Interface.html)
- [服务号订阅通知](https://developers.weixin.qq.com/doc/offiaccount/Subscription_Messages/intro.html)

## 6. 对 CoDone 的直接结论

| 产品能力 | 结论 |
|---------|------|
| 好友邀请 | 只能通过用户主动分享卡片 |
| 到点提醒 | 只能使用一次性订阅消息，按任务、按接收人订阅 |
| 长期自动提醒 | 工具类目不可行 |
| 群内催办 | 只能用户点击后调用 `wx.notifyGroupMembers` |
| 聊天任务卡 | 聊天工具模式增强；审核失败回退普通分享卡 |
| 服务号群发 | 不作为 MVP 通道 |
