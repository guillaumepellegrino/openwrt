#
# Copyright (C) 2024 Guillaume Pellegrino
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=memtrace
PKG_VERSION:=1.6.1
PKG_RELEASE:=1

#PKG_SOURCE_PROTO:=git
#PKG_SOURCE_URL:=https://github.com/guillaumepellegrino/memtrace
#PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_DIR:=/home/sahphilog2/Workspace/memtrace

PKG_MAINTAINER:=Guillaume Pellegrino <guillaume.pellegrino@yahoo.com>
PKG_LICENSE:=GPL-2.0-or-later
PKG_LICENSE_FILES:=COPYING

# We need to keep .memtrace_hooks section in libmemtrace-agent.so
STRIP := $(TOOLCHAIN_DIR)/bin/$(TARGET_CROSS)strip --keep-section=.memtrace_hooks

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/host-build.mk

define Package/memtrace
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=Memory leak tracer
  DEPENDS:=
endef

define Package/memtrace/description
  A memory leak cross-debugger for Linux Embedded Systems.
  Simply attach to a target process with memtrace -p $$pid.
endef

define Build/Prepare
		mkdir -p $(PKG_BUILD_DIR)
		cp -r $(PKG_SOURCE_DIR)/* $(PKG_BUILD_DIR)
		$(Build/Patch)
endef

define Build/Compile
	CC="$(TARGET_CC)" \
	CFLAGS="$(TARGET_CFLAGS)" \
	LDFLAGS="$(TARGET_LDFLAGS)" \
	O="build-wrt" \
	$(MAKE) -C $(PKG_BUILD_DIR) target-all
endef

define Package/memtrace/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/scripts/memtrace-endurance $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/build-wrt/memtrace $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/build-wrt/libmemtrace-agent.so $(1)/usr/lib/
endef

define Host/Compile
	CC_HOST="$(HOSTCC)" \
	CFLAGS_HOST="$(HOST_CFLAGS)" \
	LDFLAGS_HOST="$(HOST_LDFLAGS)" \
	$(MAKE) -C $(PKG_BUILD_DIR) host-all
endef

define Host/Install
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/build-host/memtrace-server $(1)/bin/
endef

$(eval $(call HostBuild))
$(eval $(call BuildPackage,memtrace))
