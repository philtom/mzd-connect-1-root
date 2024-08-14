# [Guide] ND1/ND2 - Touchscreen in motion on latest FW, No hardware required

## Original Post
https://forum.miata.net/vb/showthread.php?t=782788

Due to popular demand I'm publishing a step-by-step guide on how to access the engineering menu [allowing to activate touchscreen in motion] on ND1/ND2 with the latest firmware version.

This method does not require you to disassemble the car or install any third party hardware.

TESTED ON LATEST V74 FIRMWARE

What this gives you:
- Ability to disable "Touchscreen in Motion" restrictions (allows to use touchscreen at any time)
- Access to Linux root shell
- By extension to the previous point, ability to install Mazda AIO Tweaks

Disclaimer
IMO procedure is very safe and not very involved, however, as always with things like this, I am not responsible for any damages to the car / infotainment system and everything you do is AT YOUR OWN RISK.

Now that we’ve got this out of the way, let’s begin.

This method was discovered a while ago and shared on Mazda3Revolution forums. I'm just putting this together as a text guide for others to follow. It's referred to as "MP3 Hack" or "XSS Exploit".

How does this work
A bit of technical background- head unit (also known as CMU- Connectivity Master Unit) on ND1 and ND2 is an embedded computer. Not in a way your engine control unit or pretty much any other piece of electronics in a car is called a "computer", but a bit more literally.
It's running Linux operating system, and has some programs installed on it, similar to a server or your conventional "personal computer". The "UI" or what you actually see on a screen is just a web browser, with a "locally served“ webpage rendered on it.
Webpages, apart from visual elements, may contain executable code, JavaScript. Normally any visual elements on a webpage should be "sanitised", meaning visual element shall NOT be treated as potentially executable code (e.g. I can't write a bit of JavaScript code in this post, and make your browser execute it), but the company (not to be named) that has developed Mazda's CMU may have skipped this Web Dev 101 class and caused this vulnerability which is called XSS (Cross-Site-Scripting).

So, the "hack" really only does this:
1) The "title" of the .mp3 file contains JavaScript executable code
2) When you go to play that file in your car, the UI (browser on the CMU) renders the title of the song on a webpage, which in turn, due to sanitisation failure, gets executed.
3) Executable code, in this case, third-party code, performs the same operation as you could previously do manually on earlier versions of CMU firmware, "programmatically" opening the in-built diagnostics/engineering menu.
3.5) This works, because the company (not to be named) that has developed Mazda's CMU, rather than completely patching out this diagnostics menu pretty much "disabled visible buttons" allowing to access it. The menu itself is still there.

Step by step guide

[Prepare USB Payload]
1. Go to https://github.com/mzd-evo/mzd-connect-1-root and click on green “code” button and choose “download zip”.

Example Screenshot

2. This will download the archive with USB payload
3. Unzip the downloaded file
4. The resulting folder “mzd-connect-1-root-main” contains all the files and folders you would later need to copy over to a USB drive

[Prepare USB Drive]
1. Pick a USB flash drive you’d like to use for this
2. Format the drive as FAT32 (This will delete all the data) and give it a recognisable name- I named mine “MAZDA”. If you’re doing this under MacOS, use Disk Utility and make sure you pick “FAT” and “Master Boot Record”.
3. Copy the CONTENTS of “mzd-connect-1-root-main” folder onto the USB Drive

How your USB Drive should look like in the end

[Run the exploit on the car]
1. Put the car in ACC mode or start the engine
2. Wait for infotainment system to boot up completely, show disclaimer, and give it 1-2 minutes to “settle”.

Get to this screen

3. Plug the USB drive into one of the USB ports

Example

4. You should see the car recognise plugged in USB drive and show it’s name in a little pop-up at the top saying “USB1/2: MAZDA (or the name of your drive) Connected”

USB Recognised

5. If you're not already on it. go to infotainment menu (list containing Bluetooth Audio, Apple CarPlay, and etc) and find an entry for your USB Drive (e.g. USB 1 - MAZDA)
6. Select it, and you should see infotainment attempt to “play” files from it.

USB Playback Started

7. If you prepared your USB correctly, you should see a white popup screen appear. On this screen tap “Open Terminal”

Exploit executed successfully

8. Wait a few seconds and you will see the standard diagnostics menu

Standard diagnostics menu
Standard diagnostics menu a few seconds later

9. A few seconds later, “Choose a script you would like to run” popup will appear.

Script Menu - 1
Script Menu - 2
Script Menu - 3

10. Tap “Next” twice, until you see “LVDS SPEED TOGGLE” in the top left corner of the pop-up window
11. Tap on it, and you should see another popup saying “LVDS Speed Restriction has been toggled: DISABLE”

Result Message

12. Tap OK, and turn the car off
13. Unplug USB

Congratulations, you have now activated touchscreen in motion. You can now go for a drive and test it out.

DO NOT CLICK ON ANYTHING ELSE IF ALL YOU WANT IS TOUCHSCREEN IN MOTION
This "setting" persists until you go into the engineering menu again and change the setting back yourself.

For those who want to poke around firmware, you may open "terminal" and operate it by connecting a USB keyboard to your USB port. The terminal is under the root user, you can set your own password for it (cmu linux user), and for further convenience setup SSH and etc.

What is this Terminal" button? What is this Linux Root Shell and how do I use it?
If you have to ask, respectfully, it's not for you.

What do other buttons do?
If you have to ask, respectfully, it's not for you.

How do I install tweaks
I might write a guide on it later, but for now it's out of scope for this post. For now I’d actually recommend against installing tweaks.
