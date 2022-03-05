# 快速易班校本化打卡

自疫情以来，易班打卡成了每天例行的繁琐任务。辅导员天天在年级群里催打卡。早些时候研究的是某高校大佬用python写的代码，还很荣幸地贡献了v3版登录api（虽然现在已经废弃了）。后来大佬的代码停止维护了，v3地址也失效了，所以在研究出新的登录api后，用php重写了一遍，当时的思路就已经转换到使用web服务器来为多人提供打卡服务上了。后来觉得php上也不方便，又用nodejs写了一遍，到现在修修改改，已经跑了一年多了。再有一年多可能我也用不着了，所以把它开源了。
## 食用方法：
* npm install
* 修改config.json
* node index

## 如何打卡？
打开[http://localhost:4500](http://localhost:4500)，填入易班账号密码，然后点击确定。此时会返回当次打卡需要填写的表单。确定表单全部填写正确后，再次点击确定。如果当次打卡成功，请收藏当前的网址。之后需要打卡时，打开收藏的网址，正常情况下稍等片刻即可自动打卡。
该程序是为了自动化而设计的，不会加载非必填的项目，不会加载文件上传、图片上传等项目。特殊的打卡任务还是请老实上易班app里搞。
本着尽量少打扰的原则，没有诸如“向指定邮箱发送打卡结果”的功能。不过打卡成功后会返回分享链接，有截图需求的可以留意该链接。

## config.json
* port:程序运行在哪个端口，默认4500
* app_key:用于加载高德地图定位组件，以实现简单方便的自定义打卡定位功能。获取方法见https://lbs.amap.com/api/javascript-api/guide/abc/prepare
* app_secret_key:同上

## /data/announcement.json
用于实现简单的公告系统。

## /data/blacklist.json
黑名单指的是一组表单id。在某些特定情况下，该id的表单不会被显示在易班app里，但是仍然会被程序检测到。所以将其放入黑名单中。

## 关于
该程序原本是为了闽江学院而定制的，但理论上能够兼容其他学校的校本化打卡。如果你的打卡失败了或者有什么不对劲，还请自行检查代码，或者发issue。
自己搭建的demo：https://yiban.doveyige.top
