include $(TOPDIR)/rules.mk

PKG_NAME:=rtl8852au
PKG_RELEASE:=1

PKG_LICENSE:=GPLv2
PKG_LICENSE_FILES:=

PKG_SOURCE_URL:=https://github.com/lwfinger/rtl8852au.git
PKG_MIRROR_HASH:=786312b7b81ecbd56642a58f4e02bb80523d7a7524485b36f8819a7e757f2a54
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2023-12-05
PKG_SOURCE_VERSION:=70bdde265b9ab002daf11d4bea1a42baa8da4325

PKG_MAINTAINER:=Andriy Utkin <hello@autkin.net>
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/rtl8852au
  SUBMENU:=Wireless Drivers
  TITLE:=The driver supports rtl8832au/rtl8852au chipsets
  DEPENDS:=+kmod-cfg80211 +kmod-usb-core +@DRIVER_11AC_SUPPORT  
  FILES:=$(PKG_BUILD_DIR)/rtl8852au.ko
  AUTOLOAD:=$(call AutoProbe,rtl8852au)
  PROVIDES:=kmod-rtl8852au
endef

NOSTDINC_FLAGS := \
	$(KERNEL_NOSTDINC_FLAGS) \
	-I$(PKG_BUILD_DIR) \
	-I$(PKG_BUILD_DIR)/include \
	-I$(STAGING_DIR)/usr/include/mac80211-backport \
	-I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211 \
	-I$(STAGING_DIR)/usr/include/mac80211/uapi \
	-include backport/backport.h \
	-Wno-error=address \
	-Wno-error=stringop-overread

NOSTDINC_FLAGS+=-DCONFIG_IOCTL_CFG80211 -DRTW_USE_CFG80211_STA_EVENT -DBUILD_OPENWRT

define Build/Compile
	+$(KERNEL_MAKE) $(PKG_JOBS) \
		M="$(PKG_BUILD_DIR)" \
		NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
		CONFIG_RTL8852AU=m \
		modules
endef

$(eval $(call KernelPackage,rtl8852au))
