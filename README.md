# Supernote Webview Instructions

First off I'd like to thank [ThePixelHunter](https://www.reddit.com/user/ThePixelHunter/) and [ta-1312](https://www.reddit.com/user/ta-1312/) for their contribution to this. I have got most of this from them.

**Is this safe?**   
It worked for me, but I only have one device so I can not say. Follow these instructions at your own risk. Please note my device is a Supernote A6X running software version 2.8.22

**Do I recommend you do this?**   
No I do not. Most people will get zero benefit from this. As I understand the Supernote team would also recommend you don't do this.

**Why would you want to update your webview?**  
If you are installing external APK's, some of them will require a newer webview than the Supernote comes with by default. Updating your webview will allow some of them to run. Likewise, websites will load better if you are using a browser like E Ink Bro.

**Will this let Obsidian run on my device?**  
No it won't, it will get you past the initial webview error, but obsidian also needs DocumentUI.apk(android files) to pick a vault location. Currently, nobody has been able to get this to work. 

**Are you reading this from the future?**  
If you are, these instructions may be outdated. Use at your own risk.

**What OS will this work on?**  
It should work on windows(untested), linux and macos. However you will need to install [adb](https://github.com/TA1312/supernote-a5x/blob/master/sideload.md) and the instructions may need a little tweaking.

**Instructions:**  
As I understand it, the Supernote will only take a webview with the package name `com.android.webview`. You need a webview with that package name and the same webview must support at least Android 8. You can find webviews that will work as of today, here: https://www.apkmirror.com/apk/lineageos/android-system-webview-2/

To install the webview you must unlock root on your device. You can find scripts to do that here: A5X A6X

Run the script with this command:

**Linux & Mac**  
`bash supernoterootA6.sh root`  
or  
`bash supernoterootA5.sh root`

**Windows** (Script should work, but has not been tested)  
`./supernoterootA6.bat root`  
or  
`./supernoterootA5.bat root`

Your supernote will reboot a couple of times and you will be able to access root.

Run this script to disable the current webview:  
`adb shell pm disable com.android.webview`

Reboot to recovery  
`adb recovery`

Run these once you are in recovery:  
`adb shell busybox mount -o rw,seclabel,relatime,data=ordered,inode_readahead_blks=8 /dev/block/by-name/system /system`  
`adb shell sed -i "s/ro.debuggable=0/ro.debuggable=1/" /system/etc/prop.default`

Time to make a backup of your current webview:  
`adb pull /system/app/webview/webview.apk`

Now we push the new webview to your device:  
`adb push <new-webview-filename> /system/app/webview/webview.apk`

You should see something like this:  
`<new-webview-filename> 1 file pushed, 0 skipped.`

We need to give the new file the proper permissions to run:  
`adb shell chmod 644 /system/app/webview/webview.apk`  
`adb shell ls -la /system/app/webview/webview.apk`

You should see something like:  
`-rw-r--r-- 1 root root 96226345 <date and time> /system/app/webview/webview.apk`

Now we will reboot and install the same new webview file to your device:  
`adb reboot`  
Once rebooted:  
`adb push <new-webview-filename> /sdcard/Download/com.android.webview.apk`  
Wait for the file to finishing transferring and run:  
`adb shell pm install -t -r "/sdcard/Download/com.android.webview.apk"`

Provided that it all went well, you should have an updated running webview. Open a browser like E ink Bro to check. If it doesn't open, you may need to reboot. If it still doesn't open after this, something went wrong. You may need to restore the webview you backed up, you can follow the above instructions with the backup you made.

The last step is to unroot your device. Do so by running the same script you did in the beginning:   
**Linux & Mac**  
`bash supernoterootA6.sh unroot`  
or  
`bash supernoterootA5.sh unroot`

**Windows** (Script should work, but has not been tested)  
`./supernoterootA6.bat unroot`  
or  
`./supernoterootA5.bat unroot`

If you have issues, let me know, but keep in mind I only have so much time. :)
