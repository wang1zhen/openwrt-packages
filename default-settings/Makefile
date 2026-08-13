# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2021 Lean <coolsnowwolf@gmail.com>
# Copyright (C) 2021-2024 ImmortalWrt.org

include $(TOPDIR)/rules.mk

PKG_NAME:=default-settings
PKG_RELEASE:=30

PKG_LICENSE:=GPL-2.0-only

include $(INCLUDE_DIR)/package.mk

define Package/default-settings
  SECTION:=luci
  CATEGORY:=LuCI
  TITLE:=Personal default settings for x86/64 four-port devices
  DEPENDS:=@TARGET_x86_64 +luci +luci-ssl +luci-app-uhttpd
  PKGARCH:=all
endef

define Package/default-settings/description
  Personal first-boot defaults for x86/64 devices with eth0 through eth3.
endef

define Package/default-settings-chn
  $(Package/default-settings)
  TITLE+= (Simplified Chinese locale)
  DEPENDS:=+default-settings +@LUCI_LANG_zh_Hans +luci-i18n-base-zh-cn
endef

define Build/Compile
endef

define Package/default-settings/install
	$(INSTALL_DIR) $(1)/etc/uci-defaults
	$(INSTALL_BIN) ./files/99-default-settings $(1)/etc/uci-defaults/
endef

define Package/default-settings-chn/install
	$(INSTALL_DIR) $(1)/etc/uci-defaults
	$(INSTALL_BIN) ./files/99-default-settings-chinese $(1)/etc/uci-defaults/
endef

$(eval $(call BuildPackage,default-settings))
$(eval $(call BuildPackage,default-settings-chn))
