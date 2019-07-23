#
# Copyright 2019 Ismael Luceno
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

PKG_MAINTAINER := Ismael Luceno <ismael@linux.com>
PKG_LICENSE := GPL-2.0-or-later
PKG_LICENSE_FILES := COPYING

PKG_NAME := axel

PKG_VERSION := 2.17.5
PKG_HASH := 5c5c682617d498071ff9083029fcecc2c2fdeeedc366c35d2bdf8663355616cc

PKG_SOURCE := $(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL := https://github.com/axel-download-accelerator/axel/releases/download/v$(PKG_VERSION)


PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(BUILD_VARIANT)/$(PKG_NAME)-$(PKG_VERSION)


define Package/axel/Default
  SECTION := net
  CATEGORY:= Network
  SUBMENU := File Transfer
  TITLE   := Lightweight CLI download accelerator
  URL     := https://github.com/axel-download-accelerator/axel
endef

define Package/axel
$(Package/axel/Default)
  DEPENDS += +libopenssl
  TITLE   += (with SSL support)
  VARIANT := ssl
  ALTERNATIVES := 300:/usr/bin/axel:/usr/bin/axel-ssl
endef

define Package/axel-nossl
$(Package/axel/Default)
  TITLE   += (without SSL support)
  VARIANT :=nossl
  ALTERNATIVES := 300:/usr/bin/axel:/usr/bin/axel-nossl
endef

define Package/axel/Default/description
 Axel tries to accelerate the download process by using multiple
 connections per file, and can also balance the load between different
 servers.

 Axel tries to be as light as possible, so it might be useful on
 byte-critical systems.

 Axel supports HTTP, HTTPS, FTP and FTPS protocols.
endef

define Package/axel/description
$(Package/axel/Default/description)
 This package was built with SSL support.
endef

define Package/axel-nossl/description
$(Package/axel/Default/description)
 This package was built without SSL support.
endef

CONFIGURE_ARGS+= \
	--disable-rpath

# ac_cv_prog_cc_c99 is required because the C99 test provided by
# autoconf relies on wchar_t.
CONFIGURE_VARS += \
	ac_cv_prog_cc_c99=-std=c99

ifeq ($(BUILD_VARIANT),ssl)
	CONFIGURE_ARGS+= \
		--with-ssl
else
	CONFIGURE_ARGS+= \
		--without-ssl
endif

define Package/axel/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/axel $(1)/usr/bin/axel-$(BUILD_VARIANT)
endef
Package/axel-nossl/install = $(Package/axel/install)

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/package.mk

$(eval $(call BuildPackage,axel))
$(eval $(call BuildPackage,axel-nossl))
