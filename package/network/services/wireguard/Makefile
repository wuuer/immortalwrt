#
# Copyright (C) 2016-2019 Jason A. Donenfeld <Jason@zx2c4.com>
# Copyright (C) 2016 Baptiste Jonglez <openwrt@bitsofnetworks.org>
# Copyright (C) 2016-2017 Dan Luedtke <mail@danrl.com>
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=wireguard

PKG_VERSION:=1.0.20211208
PKG_RELEASE:=1

PKG_SOURCE:=wireguard-linux-compat-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=https://git.zx2c4.com/wireguard-linux-compat/snapshot/
PKG_HASH:=c0e607138a17daac656f508d8e63ea3737b5221fa5d9288191ddeb099f5a3b92

PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYING

PKG_BUILD_DIR:=$(KERNEL_BUILD_DIR)/wireguard-linux-compat-$(PKG_VERSION)
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

# WireGuard's makefile needs this to know where to build the kernel module
export KERNELDIR:=$(LINUX_DIR)

include $(INCLUDE_DIR)/package.mk

define Package/wireguard/Default
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=VPN
  URL:=https://www.wireguard.com
  MAINTAINER:=Jason A. Donenfeld <Jason@zx2c4.com>
endef

define Package/wireguard/Default/description
  WireGuard is a novel VPN that runs inside the Linux Kernel and utilizes
  state-of-the-art cryptography. It aims to be faster, simpler, leaner, and
  more useful than IPSec, while avoiding the massive headache. It intends to
  be considerably more performant than OpenVPN.  WireGuard is designed as a
  general purpose VPN for running on embedded interfaces and super computers
  alike, fit for many different circumstances. It uses UDP.
endef

define Package/wireguard
  $(call Package/wireguard/Default)
  TITLE:=WireGuard meta-package
  DEPENDS:=+wireguard-tools +kmod-wireguard
endef

include $(INCLUDE_DIR)/kernel-defaults.mk
include $(INCLUDE_DIR)/package-defaults.mk

define Build/Compile
	$(MAKE) $(KERNEL_MAKEOPTS) M="$(PKG_BUILD_DIR)/src" modules
endef

define Package/wireguard/install
  true
endef

define Package/wireguard/description
  $(call Package/wireguard/Default/description)
endef

define KernelPackage/wireguard
  SECTION:=kernel
  CATEGORY:=Kernel modules
  SUBMENU:=Network Support
  TITLE:=WireGuard kernel module
  DEPENDS:=+IPV6:kmod-udptunnel6 +kmod-udptunnel4
  FILES:= $(PKG_BUILD_DIR)/src/wireguard.$(LINUX_KMOD_SUFFIX)
  AUTOLOAD:=$(call AutoProbe,wireguard)
endef

define KernelPackage/wireguard/description
  $(call Package/wireguard/Default/description)

  This package provides the kernel module for WireGuard.
endef

$(eval $(call BuildPackage,wireguard))
$(eval $(call KernelPackage,wireguard))
