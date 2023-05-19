---
title: Dump VollaOS vollaphone
---
## Dump android installation

Prérequis 
- Android rooté
- outils adb/fastboot

### Backup des données

`$ mkdir Documents/BackupVolla`

`$ adb backup -apk -shared -all -f /home/$USER/Documents/BackupVolla/backup_android.ab`

indiquer un mot de passe sur le vollaphone

### Pour restaurer les données 
`$ adb restore /home/$USER/Documents/BackupVolla/backup_android.ab`

## Backup des partitions root needed

`$ adb shell`

Obtenir la liste des partitions

`yggdrasil:/ $ ls -al /dev/block/platform/bootdevice/by-name/`

`yggdrasil:/ $ mkdir /sdcard/dump_android`

`yggdrasil:/ $ su`

`yggdrasil:/ # dd if=/dev/block/mmcblk0boot0 of=~/sdcard/dump_android/preloader_a.img`

`yggdrasil:/ # dd if=/dev/block/mmcblk0boot1 of=~/sdcard/dump_android/preloader_b.img`

`yggdrasil:/ # dd if=/dev/block/mmcblk0p3 of=~/sdcard/dump_android/recovery.img`

`yggdrasil:/ # dd if=/dev/block/mmcblk0p25 of=~/sdcard/dump_android/boot.img`

`yggdrasil:/ # dd if=/dev/block/mmcblk0p27r of=~/sdcard/dump_android/dtbo.img`

`yggdrasil:/ # dd if=/dev/block/mmcblk0p26 of=~/sdcard/dump_android/logo.img`

`yggdrasil:/ # dd if=/dev/block/mmcblk0p16 of=~/sdcard/dump_android/md1sp.img`

`yggdrasil:/ # dd if=/dev/block/mmcblk0p15 of=~/sdcard/dump_android/md1img.img`

`yggdrasil:/ # dd if=/dev/block/mmcblk0p17 of=~/sdcard/dump_android/spmfw.img`

`yggdrasil:/ # dd if=/dev/block/mmcblk0p24 of=~/sdcard/dump_android/lk2.img`

`yggdrasil:/ # dd if=/dev/block/mmcblk0p19 of=~/sdcard/dump_android/sspm_2.img`

`yggdrasil:/ # dd if=/dev/block/mmcblk0p29 of=~/sdcard/dump_android/tee2.img`

`yggdrasil:/ # dd if=/dev/block/mmcblk0p23 of=~/sdcard/dump_android/lk.img`

`yggdrasil:/ # dd if=/dev/block/mmcblk0p18 of=~/sdcard/dump_android/sspm_1.img`

`yggdrasil:/ # dd if=/dev/block/mmcblk0p28 of=~/sdcard/dump_android/tee1.img`

`yggdrasil:/ # dd if=/dev/block/mmcblk0p31 of=~/sdcard/dump_android/system.img`

`yggdrasil:/ # dd if=/dev/block/mmcblk0p30 of=~/sdcard/dump_android/vendor.img`

`yggdrasil:/ # exit`

`yggdrasil:/ $ exit`

`$ adb pull /sdcard/dump_android /home/$USER/Documents/BackupVolla/dump_android`

`$ mkdir /home/$USER/Documents/BackupVolla/dump_android/META-INF`

`$ mkdir /home/$USER/Documents/BackupVolla/dump_android/META-INF/com`

`$ mkdir /home/$USER/Documents/BackupVolla/dump_android/META-INF/com/google`

`$ mkdir /home/$USER/Documents/BackupVolla/dump_android/META-INF/com/google/android`

`$ touch /home/$USER/Documents/BackupVolla/dump_android/META-INF/com/google/android/updater-script`

`$ wget https://debian-facile.org/images/file-R25ac9925cdd9fe07763413ca6210a1c2 -O /home/$USER/Documents/BackupVolla/dump_android/META-INF/com/google/android/update-binary`

Editer /home/$USER/Documents/BackupVolla/dump_android/META-INF/com/google/android/updater-script

`package_extract_file("preloader_a.img", "/dev/block/mmcblk0boot0");

package_extract_file("preloader_b.img", "/dev/block/mmcblk0boot1");

package_extract_file("recovery.img", "/dev/block/mmcblk0p3");

package_extract_file("boot.img", "/dev/block/mmcblk0p25");

package_extract_file("dtbo.img", "/dev/block/mmcblk0p27r");

package_extract_file("logo.img", "/dev/block/mmcblk0p26");

package_extract_file("md1sp.img", "/dev/block/mmcblk0p16");

package_extract_file("md1img.img", "/dev/block/mmcblk0p15");

package_extract_file("spmfw.img", "/dev/block/mmcblk0p17");

package_extract_file("lk2.img", "/dev/block/mmcblk0p24");

package_extract_file("sspm_2.img", "/dev/block/mmcblk0p19");

package_extract_file("tee2.img", "/dev/block/mmcblk0p29");

package_extract_file("lk.img", "/dev/block/mmcblk0p23");

package_extract_file("sspm_1.img", "/dev/block/mmcblk0p18");

package_extract_file("tee1.img", "/dev/block/mmcblk0p28");

package_extract_file("system.img", "/dev/block/mmcblk0p31");

package_extract_file("vendor.img", "/dev/block/mmcblk0p30");

delete_recursive("/data/dalvik-cache/");

delete_recursive("/cache/dalvik-cache/");

delete_recursive("/cache");

ui_print("Done!");`

## Création du zip

`$ cd /home/$USER/Documents/BackupVolla/dump_android/dump_android`

`$ zip -r9 /home/$USER/dump_android.zip *`

## Signature

`$ wget https://debian-facile.org/images/file-Rb3866f5849cc53ea3c0468a27a5dd51f -O SignApk.zip`

`$ cd SignApk`

`$ sudo apt-get install apksigner`

`$ apksigner sign --cert testkey.x509.pem --key testkey.pk8 --min-sdk-version 15 --verbose ~/dump_android.zip`