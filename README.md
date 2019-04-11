1. ##  Linux 环境配置
1.升级git版本（要求git 版本2.6.0以上，解决格式化日期错误：fatal: unknown date format format:%Y-%M-%D %H:%M:%S
ERROR: Unable to retrieve changeset）；  
2.配置jenkins上的git路径（全局工具配置/usr/local/git-2.8.3/bin/git）；  
3.安装android-ndk和sdk，jenkins上设置路径PATH；  
4.jenkins设置git路径PATH。/usr/local/git-2.8.3/bin；  
5.安装ruby，通过rubygem安装fir-cil；  
6.安装gradle，配置路径；  
7.jenkins安装fir-plugin插件；
2. ## Android 开发版本配置
Androidstudio 3.3.1,gradle 3.3.1  
compileSdkVersion: 27,  
buildToolsVersion: "28.0.3",  
minSdkVersion    : 16,  
targetSdkVersion : 27,  
- ## 现在我们是布到Linux 上打包，所以local.properties 指向的路径会不一样。 
ndk.dir=/usr/local/android-ndk-r19c  
sdk.dir=/usr/local/android-sdk-linux

3. ## Jenkins 界面配置
## 构建  
 Invoke Gradle script>Use Gradle Wrapper 选中Make gradlew executable  
 Tasks: clean  
        assenbleRease
## Command 

 fir login 424f9722ed0fb80654fc1bb1f2ec784e
    
 fir me 
 
 export LANG="en_US.UTF-8"
 
 
 git --version
 
 git config log.date  "format:%Y-%m-%d %H:%M:%S"
 
 fir publish app/build/outputs/apk/gjmetal/release/*.apk -c "$(git log -5 --pretty=format:'.%s  （%an,%cd）' --abbrev-commit | awk -F ':' '{print NR " " $0 }')"
