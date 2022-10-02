---
title: '利用Github Action自动续约Freenom域名 '
top: false
cover: false
toc: true
mathjax: true
date: 2021-07-02 16:43:38
password:
summary: Github Action自动续约Freenom域名
tags:
- GitHub
- 域名
- Freenom
- 代理
categories:
- Linux
---









![](https://b3logfile.com/bing/20191227.jpg?imageView2/1/w/960/h/540/interlace/1/q/100)

# Freenom

## 项目地址

+ [winkxx/freenom: Freenom域名自动续期。Freenom domain name renews automatically. (github.com)](https://github.com/winkxx/freenom)
+ ##### 注意：GitHub 官方不允许使用 GitHub Action 做签到或者续期类应用，否则会封禁项目甚至封号，故为项目能长期维护下去，应 GitHub 官方要求， 本项目已经移除 Action 方式的应用，望周知。已经 fork 使用的，可以尽快将项目转移到自己的 VPS 上，推荐通过 Docker 部署， 或者直接搬运到腾讯云函数部署。本项目依然长期维护。

## 使用

+ 该版本是带有Action版本的fork版(不建议使用吧)
+ 所以建议用腾讯云函数吧，只列举腾讯云的用法

### ?  通过腾讯云函数（SCF）部署

<hr>

#### 1、下载 SCF 版本的压缩包

此版本为特别版，支持通过腾讯云函数部署，与主分支版本不兼容，版本号为`v0.3_scf`，下载地址：
[https://github.com/luolongfei/freenom/archive/refs/tags/v0.3_scf.zip](https://github.com/luolongfei/freenom/archive/refs/tags/v0.3_scf.zip)

下载后解压到你能找到的任意目录，你将得到一个文件夹，后期将通过文件夹的形式上传到腾讯云函数。
2、创建腾讯云函数

直接访问腾讯云函数控制台创建云函数： [https://console.cloud.tencent.com/scf/list-create](https://console.cloud.tencent.com/scf/list-create) ，
按照下图所示的说明进行创建。如果无法看清图片，可访问： [https://github.com/luolongfei/freenom/blob/master/resources/screenshot/scf.png](https://github.com/luolongfei/freenom/blob/master/resources/screenshot/scf.png)
或者 [https://z3.ax1x.com/2021/06/01/2nKCF0.png](https://z3.ax1x.com/2021/06/01/2nKCF0.png) 查看原图。

[![scf01](https://z3.ax1x.com/2021/06/01/2nKCF0.png)](https://imgtu.com/i/2nKCF0)

按照上图所示部署完成后，可以点击云函数的名称进入云函数管理画面，管理画面往下翻可看到`部署`与`测试`按钮，点击`测试`，稍等几秒钟，即可看到输出日志，
根据输出日志判断配置以及部署是否正确。

[![scf02](https://z3.ax1x.com/2021/06/01/2nGZ3q.png)](https://imgtu.com/i/2nGZ3q)

*有关腾讯云函数部署的内容结束。*

## 通知

+ 人懒不想弄通知
+ 怎么弄？添加云函数中的环境即可
  

| 变量名 | 含义 | 默认值 | 是否必须 | 备注 |
| :---: | :---: | :---: | :---: | :---: |
| FREENOM_USERNAME | Freenom 账户 | - | 是 | 只支持邮箱账户，如果你是使用第三方社交账户登录的用户，请在 Freenom 管理页面绑定邮箱，绑定后即可使用邮箱账户登录 |
| FREENOM_PASSWORD | Freenom 密码 | - | 是 | 某些特殊字符可能需要转义，详见`.env`文件内注释 |
| MULTIPLE_ACCOUNTS | 多账户支持 | - | 否 | 多个账户和密码的格式必须是“`<账户1>@<密码1>\|<账户2>@<密码2>\|<账户3>@<密码3>`”，注意不要省略“<>”符号，否则无法正确匹配。如果设置了多账户，上面的`FREENOM_USERNAME`和`FREENOM_PASSWORD`可不设置 |
| MAIL_USERNAME | 机器人邮箱账户 | - | 是 | 支持`Gmail`、`QQ邮箱`以及`163邮箱`，尽可能使用`163邮箱`或者`QQ邮箱`而非`Gmail`。因为谷歌的安全机制，每次在新设备登录 `Gmail` 都会先被限制，需要手动解除限制才行。具体的配置方法参考「 [配置发信邮箱](#--配置发信邮箱) 」 |
| MAIL_PASSWORD | 机器人邮箱密码 | - | 是 | `Gmail`填密码，`QQ邮箱`或`163邮箱`填授权码 |
| TO | 接收通知的邮箱 | - | 是 | 你自己最常用的邮箱，推荐使用`QQ邮箱`，用来接收机器人邮箱发出的域名相关邮件 |
| MAIL_ENABLE | 是否启用邮件推送功能 | true | 否 | `true`：启用`false`：不启用默认启用，如果设为`false`，不启用邮件推送功能，则上面的`MAIL_USERNAME`、`MAIL_PASSWORD`、`TO`变量变为非必须，可不设置 |
| TELEGRAM_CHAT_ID | 你的`chat_id` | - | 否 | 通过发送`/start`给`@userinfobot`可以获取自己的`id` |
| TELEGRAM_BOT_TOKEN | 你的`Telegram bot`的`token` | - | 否 ||
| TELEGRAM_BOT_ENABLE | 是否启用`Telegram Bot`推送功能 | false | 否 | `true`：启用`false`：不启用默认不启用，如果设为`true`，则必须设置上面的`TELEGRAM_CHAT_ID`和`TELEGRAM_BOT_TOKEN`变量 |
| NOTICE_FREQ | 通知频率 | 1 | 否 | `0`：仅当有续期操作的时候`1`：每次执行 |

*更多配置项含义，请参考`.env`文件中的注释。*
### ?  配置发信邮箱

下面分别介绍`Gmail`、`QQ邮箱`以及`163邮箱`的设置，你只用看自己需要的部分。注意，`QQ邮箱`与`163邮箱`均使用账户加授权码的方式登录，
`谷歌邮箱`使用账户加密码的方式登录，请知悉。另外还想吐槽一下，国产邮箱你得花一毛钱给邮箱提供方发一条短信才能拿到授权码。

*（点击即可展开或收起）*

<details>
    <summary>设置Gmail</summary>
<br>

1、在`设置>转发和POP/IMAP`中，勾选

- 对所有邮件启用 POP
- 启用 IMAP

![gmail配置01](https://s2.ax1x.com/2020/01/31/13tKsg.png "gmail配置01")

然后保存更改。

2、允许不够安全的应用

登录谷歌邮箱后，访问 [谷歌权限设置界面](https://myaccount.google.com/u/0/lesssecureapps?pli=1&pageId=none) ，启用允许不够安全的应用。

![gmail配置02](https://s2.ax1x.com/2020/01/31/1392KH.png "gmail配置02")

另外，若遇到提示

> 不允许访问账户

登录谷歌邮箱后，去 [gmail的这个界面](https://accounts.google.com/b/0/DisplayUnlockCaptcha) 点击允许。这种情况较为少见。

---

</details>

<details>
    <summary>设置QQ邮箱</summary>
<br>

在`设置>账户>POP3/IMAP/SMTP/Exchange/CardDAV/CalDAV服务`下，开启`POP3/SMTP服务`

![qq邮箱配置01](https://s2.ax1x.com/2020/01/31/13cIKA.png "qq邮箱配置01")

此时坑爹的QQ邮箱会要求你用手机发送一条短信给腾讯，发送完了点一下`我已发送`

![qq邮箱配置02](https://s2.ax1x.com/2020/01/31/13c4vd.png "qq邮箱配置02")

然后你就能看到你的邮箱授权码了，使用邮箱账户加授权码即可登录，记下授权码

![qq邮箱配置03](https://s2.ax1x.com/2020/01/31/13cTbt.png "qq邮箱配置03")

![qq邮箱配置04](https://s2.ax1x.com/2020/01/31/13coDI.png "qq邮箱配置04")

---

</details>

<details>
    <summary>设置163邮箱</summary>
<br>

在`设置>POP3/SMTP/IMAP`下，开启`POP3/SMTP服务`和`IMAP/SMTP服务`并保存

![163邮箱配置01](https://s2.ax1x.com/2020/01/31/13WKZn.png "163邮箱配置01")

![163邮箱配置02](https://s2.ax1x.com/2020/01/31/13WQI0.png "163邮箱配置02")

现在点击侧边栏的`客户端授权密码`，并获取授权码，你看到画面可能和我不一样，因为我已经获取了授权码，所以只有`重置授权码`按钮，这里自己根据网站提示申请获取授权码，网易和腾讯一样恶心，需要你用手机给它发一条短信才能拿到授权码

![163邮箱配置03](https://s2.ax1x.com/2020/01/31/13WMaq.png "163邮箱配置03")

---

</details>

### TGbot

TG说起来话长，也没多少人用的感觉。
偷懒万岁~



