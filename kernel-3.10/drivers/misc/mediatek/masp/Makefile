#######################################
# Sepcify source files
#######################################
#include $(MTK_ROOT_BUILD)/Makefile

MTK_PLATFORM := $(subst ",,$(CONFIG_MTK_PLATFORM))
MASP_CORE_DRIVER_DIR := $(srctree)/drivers/misc/mediatek/masp

ifneq (,$(filter $(CONFIG_MTK_PLATFORM), "mt6592" "mt6795" "mt6752" "mt6582" "mt6572" "mt2601"))
# v2 driver
asf_link := asfv2
else
asf_link := asf
endif
lib_obj := $(MTK_PLATFORM)/module/sec_mod.o

ifeq ($(CONFIG_MTK_SECURITY_MODULE_LITE), y)
ccflags-y += -DMTK_SECURITY_MODULE_LITE
core_obj += $(asf_link)/core/sec_ops.o
core_obj += $(asf_link)/core/alg_sha1.o
core_obj += $(asf_link)/core/sec_osal.o
ifneq ($(CONFIG_ARM64), y)
mach_obj += $(MTK_PLATFORM)/mach/arm/sec_uid.o
else
mach_obj += $(MTK_PLATFORM)/mach/arm64/sec_uid.o
endif
EXTRA_CFLAGS += -I$(src)/$(asf_link)/asf_inc
else
################################################
#[core_obj] include all the files under asf/core
################################################
include $(MASP_CORE_DRIVER_DIR)/$(asf_link)/Makefile.core

###################################################################
#[mach_obj][HACC]: if TEE is enabled, HACC is done in secure world.
###################################################################
ifeq ($(CONFIG_TRUSTONIC_TEE_SUPPORT),y)
ccflags-y += -I$(srctree)/arch/arm/mach-$(MTK_PLATFORM)/include/trustzone/sec/Tlsec/inc
ccflags-y += -I$(srctree)/drivers/misc/mediatek/mach/$(MTK_PLATFORM)/include/trustzone/sec/Tlsec/inc
include $(srctree)/drivers/misc/mediatek/gud/Makefile.include

mach_obj += $(MTK_PLATFORM)/mach/hacc_tee_req.o
mach_obj += $(MTK_PLATFORM)/mach/hacc_tee.o
mach_obj += $(MTK_PLATFORM)/mach/hacc_service.o
else
include $(MASP_CORE_DRIVER_DIR)/$(MTK_PLATFORM)/mach/Makefile.mach
endif

##################################################################################
#[lib_obj] crypto library. if custom auth is enabled, auth algo can be customized.
##################################################################################
ifeq ($(CONFIG_CUSTOM_SEC_AUTH_SUPPORT),y)
CCCI_CUSTOM_DRIVER_DIR := $(call my-dir)$(call to-root,$(obj))mediatek/custom/out/$(call lc,$(MTK_PROJECT))/kernel/ccci
lib_obj += $(CCCI_CUSTOM_DRIVER_DIR)/cust_auth.o
else
include $(MASP_CORE_DRIVER_DIR)/$(asf_link)/Makefile.crypto
lib_obj += $(crypto_obj)
endif
endif

# BUILD-IN
obj-y += sec.o

# HEADER FILES
EXTRA_CFLAGS += -I$(src)/module

# INCLUDE ASP LIBRARY
sec-y += $(lib_obj) $(core_obj) $(mach_obj)
