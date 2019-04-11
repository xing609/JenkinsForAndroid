# JenkinsForAndroid
## Android 开发版本配置
Androidstudio 3.3.1,gradle 3.3.1  
compileSdkVersion: 27,  
buildToolsVersion: "28.0.3",  
minSdkVersion    : 16,  
targetSdkVersion : 27,  
## 现在我们是布到Linux 上打包，所以local.properties 指向的路径会不一样。 
ndk.dir=/usr/local/android-ndk-r19c  
sdk.dir=/usr/local/android-sdk-linux

## Jenkins 界面配置
# Command 

 fir login 424f9722ed0fb80654fc1bb1f2ec784e
    
 fir me 
 
 export LANG="en_US.UTF-8"
 
 
 git --version
 
 git config log.date  "format:%Y-%m-%d %H:%M:%S"
 
 fir publish app/build/outputs/apk/gjmetal/release/*.apk -c "$(git log -5 --pretty=format:'.%s  （%an,%cd）' --abbrev-commit | awk -F ':' '{print NR " " $0 }')"
