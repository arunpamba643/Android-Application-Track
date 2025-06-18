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
2. Android Hooking Method

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

3. Use Frida Universal Bypass Scripts


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

1. Use Objection
Bypass SSL pinning:

```bash
android sslpinning disable --quiet

objection -g com.someapp.dev explore --startup-command 'android sslpinning disable --quiet'
```

2. Use Frida Universal Bypass Scripts

Bypass SSL Pinning using [android-ssl-pinning-bypass-2](https://codeshare.frida.re/@ivan-sincek/android-ssl-pinning-bypass-2) script:

```fundamental
frida -U -no-pause -l android-ssl-pinning-bypass-2.js -f com.someapp.dev

frida -U -no-pause --codeshare ivan-sincek/android-ssl-pinning-bypass-2 -f com.someapp.dev
```
3. Use Android Hooking Method
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