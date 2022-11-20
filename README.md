# 快速易班校本化打卡

自疫情以来，易班打卡成了每天例行的繁琐任务。辅导员天天在年级群里催打卡。早些时候研究的是某高校大佬用python写的代码（[项目地址](https://github.com/xlc520/yiban_auto_submit)），还很荣幸地贡献了v3版登录api（虽然现在已经废弃了）。后来大佬的代码停止维护了，v3地址也失效了，所以在研究出新的登录api后，用php重写了一遍，当时的思路就已经转换到使用web服务器来为多人提供打卡服务上了。后来觉得php上也不方便，又用nodejs写了一遍，到现在修修改改，已经跑了一年多了。再有一年多可能我也用不着了，所以把它开源了。

然而很遗憾的是距开源已经过去了8个月，还是没能摆脱易班打卡（悲）
而且在10月中旬竟然还有人发issue，索性把整个项目重新粉刷一下。结果就是鸽了一个月（ 不过新版总算是在11月20号搞定了。

## 食用方法（针对Windows）：
1. 安装最新的nodejs。打开[http://nodejs.cn/download/](http://nodejs.cn/download/)，点击“Windows 安装包”，下载完毕后运行安装包以安装nodejs。
2. 下载源码zip压缩包，解压，打开解压后含有代码的文件夹，按住Shift不放并右键文件夹空白处，在出现的菜单中选择“在此处打开命令窗口”或者“在此处打开powershell窗口”。
3. 在出现的黑色窗口中输入`npm install`，并回车。
4. 修改config.js：根据[config.js](https://github.com/yige233/fast_yiban#configjson)一节的指导，获取`app_key`和`app_secret_key`（即安全密钥），并填入config.js中。
5. 在步骤3的黑色窗口中输入`node index`，并回车。

其他系统下的操作方法类似。

## 如何打卡？
打开[http://localhost:4500](http://localhost:4500)，选择`登录易班`，填入易班账号密码，然后点击登录。登录状态可以点击`程序日志`查看。如果显示成功登录，那么请等待程序获取到最新的表单。点击`打卡表单`，确认表单正常后，即可开始填写表单。确定表单全部填写正确后，点击`提交任务`即可开始打卡。期间可以随时到`程序日志`页面查看执行日志。如果当次打卡成功，请收藏当前的网址。之后需要打卡时，打开收藏的网址，正常情况下稍等片刻即可自动打卡。

~~该程序是为了自动化而设计的，不会加载非必填的项目，~~不会加载文件上传~~、图片上传~~等项目。特殊的打卡任务还是请老实上易班app里搞。

继支持上传图片后，又支持了上传文件和签名图。

现在服务器提供2种上传文件的方式：1是选择文件，直接上传；2是填入文件的url和文件名，服务器通过该url上传图片。可以选择该文件仅用于本次打卡，或者可用于自动打卡。注意：只有当所有文件都选择了“可用于自动打卡”，自动打卡才能正常使用。

原来的`通过改变url指向的图片，实现不改变打卡数据的情况下改变打卡内容`的功能没了，因为我觉得它的成本和收益不对等。不如重新填一遍表来得快

本着尽量少打扰的原则，没有诸如`向指定邮箱发送打卡结果`的功能。不过打卡成功后会返回分享链接，有截图需求的可以留意该链接。

## config.js

配置文件从json格式变成js了，但是并没有增加修改它的难度。反而由于允许注释，可读性更高了。

每项配置都有注释。这里只列3项常用的：

```
  //port是程序运行的端口
  port = 4500,

  //app_key和app_secret_code用于加载地图选点组件，获取方法见https://lbs.amap.com/api/javascript-api/guide/abc/prepare，网页中提到的Key即是app_key，安全密钥即是app_secret_code
  app_key = "",
  app_secret_code = "",
```

## /public/res/announcement.json
用于实现简单的公告系统。由原来的`/data/announcement.json`迁移而来。

## ~~/data/blacklist.json~~
黑名单仍然存在，但是没有独立的配置文件了，而是合并到了`public/js/index.html.js`的`App`类里。修改这个类就可以增删黑名单。

## 更新记录

* 2022.11.20：大更新。以后应该就是在这个的基础上小修小补了。

  * 前端：有了新的ui；新的渲染表单方式；可以记录用户的操作以及服务器返回的信息
  * 后端：使用了WebSocket来和前端通信；支持各种文件上传；精简了http api（因为都使用websocket了）
  * 写了半天发现没啥好写的。。大概是想写的都写在源码的注释里了

## 关于
该程序原本是为了闽江学院而定制的，但理论上能够兼容其他学校的“易班校本化”打卡。也就是说如果你们学校不是用“易班校本化”来打卡的，本工具对你来说就毫无用处了。如果你的打卡失败了或者有什么不对劲，还请自行检查代码，或者发issue。

自己搭建的demo：[https://yiban.doveyige.top](https://yiban.doveyige.top)

前端太丑：没办法，能力有限（）能用就行了，要啥自行车

可能存在移动网络环境下打不开这个demo的问题，那是因为服务器地址在海外，移动墙中墙把我的网站风控了，导致打开极其缓慢（等到我忍无可忍了，一定去套cdn
