# [XiaoV](https://github.com/b3log/xiaov) [![Build Status](https://img.shields.io/travis/b3log/xiaov.svg?style=flat)](https://travis-ci.org/b3log/xiaov)

## 简介

XiaoV（小薇）是一个用 Java 写的 QQ 聊天机器人 Web 服务，可以用于社群互动：

* 监听多个 QQ 群消息，发现有“感兴趣”的内容时通过[图灵机器人](http://www.tuling123.com)进行智能回复
* 监听到的 QQ 群消息可以配置推送到论坛某个接口上，以实现论坛弹幕或者动态聚合效果，请看[实例](https://hacpai.com/community)
* 在论坛代码中调用小薇进行 QQ 消息推送，比如论坛有新帖时自动推送到 QQ 群
* 加小薇为好友后可通过暗号（key）让她群发消息

总之，如果你需要一个连通 QQ 群和论坛的机器人，小薇是个不错的选择 :smirk:

### 作者

小薇她爸 [Daniel](https://github.com/88250)，妈 [Vanessa](https://github.com/Vanessa)，其他好心人可以在[这里](https://github.com/b3log/xiaov/graphs/contributors)看到。

## FAQ

### 为什么要单独做成一个 Web 服务，而不是一个依赖 jar？
 
做成依赖库的话会随应用部署，从开发的角度是比较方便，但有个致命的问题是应用一般是部署在云端，而登录扫码是在本地，这样会造成 QQ 的异地登录，导致很多问题。

所以需要将小薇部署在本地，保证用手机和小薇启动后 QQ 不出现异地登录。但是这也需要解决一个问题，即需要为小薇提供“内网穿透”的能力，比如使用 ngrok，具体可参考[这里](https://hacpai.com/article/1458787368338)。

### 为什么会出现“发送失败，Api返回码[1202]”？

这个问题是因为 QQ 服务器判断消息有问题时的返回，具体可关注这个 [issue](https://github.com/ScienJus/smartqq/issues/11)。

## 启动

1. 安装好 Java 1.7+、Maven 2+
2. Clone 本项目，并在项目根目录上执行 `mvn jetty:run`

这样就可以使用默认的配置文件启动小薇了。

## 配置

配置文件主要是 src/main/resources/xiaov.properties：

* api.key 定义了论坛调用小薇时用于验证的口令
* turing.api & turing.key 定义了图灵机器人的 API 地址和口令
* qq.bot.name 定义了机器人的名字，这个主要是用于识别群消息是否“感兴趣”，比如对于群消息：“小薇，你吃过饭了吗？”包含了机器人的名字，机器人就对其进行处理
* qq.bot.key 定义了管理 QQ（加了机器人为好友的 QQ）发过来的消息群发的口令，需要消息开头是这个口令，验证过后才会群发后面的消息内容
* qq.bot.pushGroups 定义了群发的群名，用 `,` 分隔多个群
* bot.follow.keywords 定义了监听群消息时的关键词，碰到这些词就做处理，比如对于群消息：“如何能在 3 天内精通 Java 呢？”包含了关键词 Java，机器人就对其进行处理
* forum.api & forum.key 定义了论坛 API 地址和口令，小薇会将所有监听到的消息通过该 API 转发到论坛

## API

### 论坛推送 QQ 群

* 功能：小薇提供给论坛调用的 HTTP 接口，用于将论坛的内容推送到 QQ 群
* URL：/qq
* Method：POST
* Body：key={api.key}&msg={msgcontent}

### QQ 群推送论坛

* 功能：由论坛提供给小薇调用的 HTTP 接口，用于将 QQ 群消息推送到论坛（这个接口是论坛实现的，这里是给出小薇的调用方式和参数）
* URL：{forum.api}
* Method：POST
* Body：key={forum.key}&msg={msgcontent}&user={hexuserid}

## 鸣谢

小薇的诞生离不开以下开源项目/产品服务：

* [Smart QQ Java](https://github.com/ScienJus/smartqq)：封装了 SmartQQ（WebQQ）的 API，完成 QQ 通讯实现
* [图灵机器人](http://www.tuling123.com)：给予了小薇抖机灵的能力....
* [Latke](https://github.com/b3log/latke)：简洁高效的 Java Web 框架 
