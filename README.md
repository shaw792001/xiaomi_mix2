n=boot1.img
p=/sdcard/backups
n2=${p}/${n}
cd boot
../repack_ramdisk ramdisk1  boot.img-ramdisk.cpio.gz

cd ..


./mkbootimg --kernel boot/boot.img-kernel --ramdisk boot/boot.img-ramdisk.cpio.gz --cmdline 'androidboot.console=ttyMSM0 androidboot.hardware=qcom user_debug=31 msm_rtb.filter=0x37 ehci-hcd.park=3 service_locator.enable=1 swiotlb=2048 buildvariant=userdebug' -o ${n}
adb push ${n} ${p}
adb root
adb shell  dd if=${n2} of=/dev/block/bootdevice/by-name/boot bs=4096
adb reboot
