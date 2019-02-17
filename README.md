# Custom kernel for Xiaomi Redmi Note 2 (Hermes and Hennesy)
# Kernel version 3.10.72
# Vendor Vanzo (ALPS-MP-M0.MP11-V1_VZ6795_LWT_M)
# Supports miui library
# dispsys,videox,cmdq,m4u,jpeg,smi,gpu *** taken from the original kernel temporarily
=========================================================================
* Works:
	* LCM
	* Touch
	* CW2015
	* Sdcard
	* Wi-fi
	* BT
	* GPS
	* Button-backlight
	* Brightness
	* Leds indication
	* Rill
	* OTG
	* Sound(Speaker,Headphones)
	* Vibrator
	* Battery
	* Flashlight
	* Camera
	* Lens
    * Sensor
	* IR Blaster
	* Codec

=================================================
# BUILD
export TOP=$(pwd) 
export CROSS_COMPILE=/home/nofearnohappy/aarch64-linux-android-4.9-linaro-master/bin/aarch64-linux-android- 
mkdir -p $TOP/KERNEL_OBJ 
make -C kernel-3.10 O=$TOP/KERNEL_OBJ ARCH=arm64 MTK_TARGET_PROJECT=hermes TARGET_BUILD_VARIANT=user CROSS_COMPILE=$TOOLCHAIN ROOTDIR=$TOP hermes_defconfig 
make -C kernel-3.10 O=$TOP/KERNEL_OBJ ROOTDIR=$TOP 

# I2C

* I2C0
	* TPS65132              (003e)
	* KD_CAMERA_HW          (007f)
	* DF9761BAF             (0018) - LENS
	* CAM_CAL_DRV           (0036)

* I2C1
	* DA9210                (0068)
	* TPS6128x              (0075)

* I2C2
	* ATMEL                 (004a)
	* KD_CAMERA_HW_BUS_2    (007f)
	* FT			(0038)

* I2C3
	* AKM0991               (000c)
	* YAS537                (002e)
	* LSM6DS3_ACCEL         (006a)
	* LTR_559ALS		(0023)
	* LSM6DS3_GYRO		(0034)
	* STK3X1X               (0048)
	* BMI160_GYRO		(0066)
	* BMI160_ACC		(0068)
	* Step_couneter(BMI160_STC)		(007a)

* I2C4
	* CW2015 		(0062)

# AUTORS
* nofearnohappy
* LazyC0DEr
* Anomalchik
* Smosia
* .....
