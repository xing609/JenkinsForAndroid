##  Android集成jenkins+linux+fir+git 实现自动化打包  
* 简介： 为解决android 开发经常打包费时间的问题，决定花点时间搞下jenkins 自动化，配置好后让测试自己去打包，一劳永逸。  
目前android集成jenkins 打包主要有两种方式：1.jenkins 安装fir插件或蒲公英; 2.不使用插件，执行shell 脚本命令。  
看了一下网上大部分教程都是讲解集成插件化打包，优点是配置速度快、复杂度低，缺点是获取的日志只能取到最新的一条，无法配置日志显示格式。  
今天给大家介绍就是第二种实现方式，将打好的包直接上传到fir，并获取最新5条更新日志。为方便我们在外网也能打包就将环境直接布到linux 上，没有布在本机windows上，windows 上布的相对简单暂时不讲解。弄好的效果图：

##  Linux 环境配置
1.升级git版本（要求git 版本2.6.0以上，解决格式化日期错误：fatal: unknown date format format:%Y-%M-%D %H:%M:%S
ERROR: Unable to retrieve changeset）；  
2.配置jenkins上的git路径（全局工具配置/usr/local/git-2.8.3/bin/git）；  
3.安装android-ndk和sdk，jenkins上设置路径PATH；  
4.jenkins设置git路径PATH。/usr/local/git-2.8.3/bin；  
5.安装ruby，通过rubygem安装fir-cil；  
6.安装gradle，配置路径；  
7.jenkins安装fir-plugin插件；

  
 ## Android 开发版本配置
开发工具：Androidstudio 3.3.1,gradle 3.3.1  

---
build.gradle 文件配置  
compileSdkVersion: 27,  
buildToolsVersion: "28.0.3",  
minSdkVersion    : 16,  
targetSdkVersion : 27,

---  
现在我们是布到Linux 上打包，所以local.properties 指向的路径会不一样。 
```  
ndk.dir=/usr/local/android-ndk-r19c  
sdk.dir=/usr/local/android-sdk-linux
```

## Jenkins 界面配置
* General
  项目名称：test-android-xxx  
  丢弃旧的构建：保持构建的天数:3 >保持构建的最大个数:3  
  参数化构建过程：  
  Git Parameter>Name:Branch>Parameter Type:Branch  
* 源码管理  
  Git >Repository URL：http://xxxxxx.git（建议填项目根路径）  
  Credentials:configsrv/****  
  Branch Specifier (blank for 'any'):选一个分支
* 构建  
 Invoke Gradle script>Use Gradle Wrapper 选中Make gradlew executable  
 Tasks: clean  
        assenbleRease  
* 构建后操作  
 E-mail Notification>Recipients :512002160@qq.com(填写构建成功后的邮箱)
        
---        
* Command 脚本  
``` 
 //从fir 上复制APIToken 进行登录  
 fir login 424f9722ed0fb80654fc1bb1f2ec784e  
 fir me   
 //设置编码解决fir 抓取git日志只显示英文不显示中文的问题  
 export LANG="en_US.UTF-8"  
 //查看git 版本，此处git 版本必须在2.6.0以上才能格式化日期  
 git --version  
 //格式化日期  
 git config log.date  "format:%Y-%m-%d %H:%M:%S"  
 //将git 抓取后，直接上传到fir 上，并编号排序显示  
 fir publish app/build/outputs/apk/gjmetal/release/*.apk -c "$(git log -5 --pretty=format:'.%s  （%an,%cd）' --abbrev-commit | awk -F ':' '{print NR " " $0 }')"
```
## 技术交流  
*    欢迎加入Android 学习交流群：**413893967**
   <a target="_blank" href="https://jq.qq.com/?_wv=1027&k=5EUEsBC"><img border="0" src="http://pub.idqqimg.com/wpa/images/group.png"></a>
*    个人联系方式：512002160@qq.com 
