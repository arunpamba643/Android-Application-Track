# Dynamic Analysis

In Dynamic analysis we'll have to interact with the application. So, We'll use android emulator for the virtual device or we can use physical device also...

Tools involved in Dynamic Analysis
1. Android Emulator : Virtual Device
2. adb : Android debugging bridge (Helps to connet with the device)
3. Objection : mobile applictions exploitation tools
4. frida : Exploitation tool
5. Drozer : Export activity Exploitation tool

In Dynamic Analysis maily look for the below Vulnerabilities
1. Root detection 
2. SSL Pinning
3. Insecure Local Storage
4. Log files

## 1.  Root Detection

The apk should not be running on rooted mobiles...So, the app must contain root detection mechanism.. and if the device is rooted mobile the apk should not work\

for bypassing the root detection mechanism use the below methods,
1. Use Objection\
use the objection and explore the app using below command
```bash
objection -g <com.someapp.name> explore
```
```bash
android root disable
```
2. Use frida Universal scripts

3. Android Hooking Method

