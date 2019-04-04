# JenkinsForAndroid
## Command 

 fir login 424f9722ed0fb80654fc1bb1f2ec784e
    
 fir me 
 
 export LANG="en_US.UTF-8"
 
 
 git --version
 
 git config log.date  "format:%Y-%m-%d %H:%M:%S"
 
 fir publish app/build/outputs/apk/gjmetal/release/*.apk -c "$(git log -5 --pretty=format:'.%s  （%an,%cd）' --abbrev-commit | awk -F ':' '{print NR " " $0 }')"
