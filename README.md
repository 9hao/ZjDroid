ZjDroid
=======

Android app dynamic reverse tool based on Xposed framework.


I. ZjDroid Introduction

ZjDroid is a reverse tool based on Xposed Framewrok. It can do following tasks：
1. Dump dex from memory;

2. Reverse app protect(packer) by key pointer to BakSmali in Dalvik;

3. Monitor dangerous API;

4. Dump specific memory;

5. Obtain DEX info of APK;

6. Obtain class info of DEX file;

7. Dump Dalvik java heap info;

8. Run Lua scripts in target process.


II. ZjDroid commands

1. Obtain DEX info of APK:
am broadcast -a com.zjdroid.invoke --ei target pid --es cmd '{"action":"dump_dexinfo"}'

2. Obtain class info of DEX file：
am broadcast -a com.zjdroid.invoke --ei target pid --es cmd '{"action":"dump_class","dexpath":"*****"}'

3. Decompile DEX by memory pointer, save it to file:
am broadcast -a com.zjdroid.invoke --ei target pid --es cmd '{"action":"backsmali","dexpath":"*****"}'
This command can unpack most of popular packers(app protect)
Exception:
For ApkProtect has anti-modify, following actions are needed for unpack ApkProtect:
 a. Create a directory (e.g. /data/local) and chmod to 777;
 b. Copy zjdroid.apk to this dir, rename it to zjdroid.jar;
 c. Edit /data/data/de.robv.android.xposed.installer/conf/modules.list, set module file to zjdroid.jar;
 d. reboot the phone.

4. Dump specific DEX in memory and save it to file (in odex format which can be decompiled on PC)
am broadcast -a com.zjdroid.invoke --ei target pid --es cmd '{"action":"dump_dexfile","dexpath":"*****"}'


5. Dump specific memory to file
am broadcast -a com.zjdroid.invoke --ei target pid --es cmd '{"action":"dump_mem","start":1234567,"length":123}'

6. Dump Dalvik heap to file, the file can be analyzed by java heap analysis tool.
am broadcast -a com.zjdroid.invoke --ei target pid --es cmd '{"action":"dump_heap"}'

7. Run Lua script in target process
Call java code in Lua script.
Use case：
 a. Used to call decrypt method dynamically to do decryption.
 b. Used to trigger specific code logic.
am broadcast -a com.zjdroid.invoke --ei target pid --es cmd '{"action":"invoke","filepath":"****"}'
luajava usage：
http://www.keplerproject.org/luajava/

8. Monitor dangerous API.


III. Command result：

1. Check command result：
adb shell logcat -s zjdroid-shell-{package name}

2. Check API monitor result：
adb shell logcat -s zjdroid-apimonitor-{package name}
