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
5. Other Vulnerabilities

## 1.  Root Detection

The apk should not be running on rooted mobiles...So, the app must contain root detection mechanism.. and if the device is rooted mobile the apk should not work\

for bypassing the root detection mechanism use the below methods,
#### 1. Use Objection\
use the objection and explore the app using below command
```bash
objection -g <com.someapp.name> explore
```
```bash
android root disable
```
#### 2. Android Hooking Method

List classes and methods:

```bash
android hooking list classes

android hooking search classes com.someapp.dev
android hooking search classes 'keyword'

android hooking list class_methods 'someclass'
android hooking search methods com.someapp.dev 'someclass'
```

Hook on a class or method:

```bash
android hooking watch class 'someclass'

android hooking watch class_method 'somemethod' --dump-args --dump-backtrace --dump-return
```

Change the method's return value:

```bash
android hooking set return_value 'somemethod' 'somevalue'
```

#### 3. Use Frida Universal Bypass Scripts


Useful resources:

* [frida.re](https://frida.re/docs/home)
* [learnfrida.info](https://learnfrida.info)
* [codeshare.frida.re](https://codeshare.frida.re)
* [dweinstein/awesome-frida](https://github.com/dweinstein/awesome-frida)
* [interference-security/frida-scripts](https://github.com/interference-security/frida-scripts)
* [m0bilesecurity/Frida-Mobile-Scripts](https://github.com/m0bilesecurity/Frida-Mobile-Scripts)
* [WithSecureLabs/android-keystore-audit](https://github.com/WithSecureLabs/android-keystore-audit)


Bypass Root Detection using [android-root-detection-bypass-2](https://codeshare.frida.re/@KishorBal/multiple-root-detection-bypass/) script:

```fundamental
frida -U -no-pause -l android-root-detection-bypass.js -f com.someapp.dev

frida -U -no-pause --codeshare KishorBal/multiple-root-detection-bypass -f com.someapp.dev
```
## 2. SSL Pinning
SSL Pinning is a security mechanism which helps to prevent the application from capturing the traffic between the application and the server. It is used to ensure that the application is communicating with the intended server and not an attacker.

To bypass SSL Pinning, we can use the following methods:

#### 1. Use Objection
Bypass SSL pinning:

```bash
android sslpinning disable --quiet

objection -g com.someapp.dev explore --startup-command 'android sslpinning disable --quiet'
```

#### 2. Use Frida Universal Bypass Scripts

Bypass SSL Pinning using [android-ssl-pinning-bypass-2](https://codeshare.frida.re/@ivan-sincek/android-ssl-pinning-bypass-2) script:

```fundamental
frida -U -no-pause -l android-ssl-pinning-bypass-2.js -f com.someapp.dev

frida -U -no-pause --codeshare ivan-sincek/android-ssl-pinning-bypass-2 -f com.someapp.dev
```
#### 3. Use Android Hooking Method
(same as root detection bypass)
List classes and methods:

```bash
android hooking list classes

android hooking search classes com.someapp.dev
android hooking search classes 'keyword'

android hooking list class_methods 'someclass'
android hooking search methods com.someapp.dev 'someclass'
```

Hook on a class or method:

```bash
android hooking watch class 'someclass'

android hooking watch class_method 'somemethod' --dump-args --dump-backtrace --dump-return
```

Change the method's return value:

```bash
android hooking set return_value 'somemethod' 'somevalue'
```

## 3. Insecure Local Storage

Search for files and directories from the root directory:

```bash
find / -iname '*keyword*'
```

Search for files and directories in the app specific directories (run `env` in [Objection](#9-objection)):

```bash
cd /data/user/0/com.someapp.dev/

cd /storage/emulated/0/Android/data/com.someapp.dev/

cd /storage/emulated/0/Android/obb/com.someapp.dev/
```

If you want to download a whole directory from your Android device, see section [Download/Upload Files and Directories](#downloadupload-files-and-directories).

I preffer downloading the app specific directories, and then doing the [file inspection](#4-inspect-files) on my Kali Linux.

Search for files and directories from the current directory:

```bash
find . -iname '*keyword*'

for keyword in 'access' 'account' 'admin' 'card' 'cer' 'conf' 'cred' 'customer' 'email' 'history' 'info' 'json' 'jwt' 'key' 'kyc' 'log' 'otp' 'pass' 'pem' 'pin' 'plist' 'priv' 'refresh' 'salt' 'secret' 'seed' 'setting' 'sign' 'sql' 'token' 'transaction' 'transfer' 'tar' 'txt' 'user' 'zip' 'xml'; do find . -iname "*${keyword}*"; done
```

#### SharedPreferences

Search for files and directories in [SharedPreferences](https://developer.android.com/reference/android/content/SharedPreferences) insecure storage directory:

```bash
cd /data/user/0/com.someapp.dev/shared_prefs/
```

The files should not be world-readable (e.g., `-rw-rw-r--` is not good, and `-rw-rw----` is good):

```bash
ls /data/user/0/com.someapp.dev/shared_prefs/ -al
```

If the production build is [debuggable](https://developer.android.com/topic/security/risks/android-debuggable), it is possible to get the read access rights to the app specific directories as a low-privileged user by leveraging `run-as` command.

Download a file from SharedPreferences as non-root:

```bash
adb exec-out run-as com.someapp.dev cat /data/user/0/com.someapp.dev/shared_prefs/somefile.xml > somefile.xml
```

SharedPreferences is unencrypted and backed up by default, and as such, should not contain any sensitive data after user logs out - it should be cleared by calling [SharedPreferences.Editor.clear\(\)](https://developer.android.com/reference/android/content/SharedPreferences.Editor#clear()). It should also be excluded from backups by specifying [dataExtractionRules](https://developer.android.com/guide/topics/data/autobackup#include-exclude-android-12) inside app's AndroidManifest.xml.

## 4. Log files
Look for the log files of the application example use the below command for the log data

```bash
adb logcat
```
Here.. for suppose\

Sensitive login information, such as usernames and passwords, was found in plaintext within ADB Logcat logs. This indicates that the application is logging credentials insecurely, which can be easily accessed by an attacker with ADB access or through malware on the device. This violates secure coding practices and poses a high security risk. It is critical to ensure that sensitive data is never logged in plaintext.\

Report it!!

## 5. Other Vulnerabilities
### 5.1. Tap Jacking 

### 5.2. Jenus Vulnerability 

### 5.3. Task Hijacking

### 5.4. Copy and paste allowing
