# How to install a custom ROM on Xiaomi Mi A2 Lite

### Prerequisites
* Official 10.3.0.0 firmware: http://bigota.d.miui.com/V10.0.3.0.PDLMIXM/daisy_global_images_V10.0.3.0.PDLMIXM_20190114.0000.00_9.0_e8d8d4a6d0.tgz
* Fastboot and ADB: https://www.xda-developers.com/google-releases-separate-adb-and-fastboot-binary-downloads/
* Offain's TWRP image: https://androidfilehost.com/?fid=1395089523397933315
* Offain's TWRP ZIP: https://androidfilehost.com/?fid=1395089523397933316
* ForceEncryption disabler: https://zackptg5.com/downloads/Disable_Dm-Verity_ForceEncrypt_04.03.19.zip
* A custom ROM ZIP, for example crDroid: https://crdroid.net/daisy

### Introduction
Installing a custom ROM on an Android phone used to be a walk in the park. However, some newer phones, like Xiaomi Mi A2 Lite, have what's called an **A/B partition layout**. This facilitates an update process on stock ROMs, however it makes it harder to install and develop custom ROMs. You can read more about A/B partitions here: https://www.xda-developers.com/how-a-b-partitions-and-seamless-updates-affect-custom-development-on-xda/  

The upshot is that Mi A2 Lite doesn't have a recovery partition. Instead, when you install TWRP, it gets installed to the boot partition. Which means that every time you install or update a ROM, you will need to re-flash the recovery ZIP. One more thing to keep in mind is that the ROM **always gets installed to another slot**. Meaning, if you're currently in the slot B, the ROM will be installed to the slot A, vice versa. 

## Installation process

#### 1. Install the stock ROM
* Install fastboot and adb on your computer
* Put all the prerequisites in one folder on your computer
* Turn off your phone, wait for 3 seconds, then press volume down and power buttons simultaneously until you see the fastboot logo (you won't miss it)
* Connect the phone to the computer (do it fast, because the phone will turn off after a few seconds without being connected to the computer)
* Back up all of the data on your phone, then go to the folder with stock firmware (*daisy_global_images_V10.0.3.0.PDLMIXM_9.0*)
* Double click on `flash_all.bat` 
* Wait for the process to be finished

#### 2. Flash the custom ROM
* After your phone has rebooted successfully, turn it off and go into the fastboot mode again. Connect the phone to the computer
* Launch the command line and go to the folder where you put all the prerequisites (`cd Folder_name`)
* Boot the recovery image: `fastboot boot twrp-daisy-3.3.0-0-offain.img`
* You will see the recovery on your phone, prompting to enter the password. Choose "Cancel"
* In the main menu, choose "Wipe", then tap "Format Data". Type "yes".
* On your computer, copy all the ZIPs you've downloaded earlier using adb: `adb push *.zip /sdcard`
* On your phone, tap "Install" and flash the files in the following order:
  * Custom ROM
  * TWRP ZIP
  * ForceEncryption Disabler
* Don't install Magisk, GApps or anything else yet, we will do that later!
* After everything is done, **DON'T TAP REBOOT YET**. Instead, go back to the main menu, choose "Reboot", change the slot (e.g. if it says "Current Slot: A", tap "Slot B", vice versa. Finally, tap "Reboot"

#### 3. Post installation
* After your phone has successfully booted into the ROM, turn your phone off, wait for a few seconds, then press Volume Up and Power buttons simultaneously for ~3 seconds. This will make your phone boot into recovery mode
* Copy anything you would like to install on your phone (Magisk, Gapps, modules, etc.) and flash it the usual way.
* Reboot your phone

## Dirty flashing / installing updates
Installing ROM updates is also somewhat complicated on A/B phones like Xiaomi Mi A2 Lite. However, if you follow these steps, you should be good to go:

* Download the update and copy it to your phone (in case of crDroid OTAs, you obviously don't need to do that since the updates are downloaded directly on your phone
* Reboot into the recovery mode and flash the ROM ZIP.
* Here's where the fun part begins: since we've just overwritten the boot partition, **we don't have TWRP anymore, so we need to flash it again.** 
* Once again, after you're done, don't tap on "Reboot" yet, instead, go back to the main menu, change the slot, and then tap "Reboot".
* After your phone has booted to the ROM, you might notice that Magisk is now gone. That's because Magisk's survival script doesn't work all that well on A/B phones.
* Reboot into recovery again, and flash Magisk and Gapps. 


Yes, developers and maintainers are aware that this process is ridiculously convoluted and makes no sense, and we're working on that. A/B devices are still relatively new and it will take some time for the developers to come up with a better way to flash custom ROMs. Be patient and use this guide. Thank you!


## FAQ
**Q:** I can't unlock my phone after flashing an update!  
**A:** Reboot into recovery and delete the file `/data/system/locksettings.db` and similiar files with different extensions (locksettings.shm, locksettings.wal, etc.)


**Q:** I can't enroll a fingerprint! When I touch the sensor, nothing happens, or it goes almost until the end and then says "Enrollment was not finished"  
**A:** This happens when the fingerprint database is full. Reboot into recovery and delete `/persist/data/finger_0_x` (finger0_1, finger0_1.bak, finger0_2, etc.)


**Q:** I still can't enroll a fingerprint  
**A:** Flash this fix: https://forum.xda-developers.com/attachment.php?attachmentid=4745543&d=1555675395

**Q:** After enabling Camera2 API, stock camera doesn't work! And Google Camera says "Unable to connect to camera"  
**A:** At the moment of writing this guide. Camera2 API doesn't work properly on custom ROMs. If this functionality is crucial for you, use Treble ROMs with Oreo base or stock ROM. 

**Q:** USB options are greyed out and I can't choose Data Transfer  
**A:** Go to Developer Settings > USB Preferences and choose "File Transfer"  


### Credits
@33bca for the device trees and TWRP
