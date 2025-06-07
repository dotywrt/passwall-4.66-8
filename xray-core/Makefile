include $(TOPDIR)/rules.mk

PKG_NAME:=xray-core
PKG_VERSION:=1.8.1
PKG_RELEASE:=1

PKG_SOURCE:=Xray-core-v1.8.1.zip
PKG_SOURCE_URL:=https://raw.githubusercontent.com/dotywrt/core/main
PKG_HASH:=1892aa75130a684934196cd6b2764f905573f5ad016251c0bf8657390d2c2062

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk

define Package/xray-core
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Custom Xray binary as /usr/bin/xray
  MAINTAINER:=DOTYWRT <admin@dotycat.com>
  DEPENDS:=+ca-bundle
endef

define Package/xray-core/description
Installs a custom prebuilt Xray binary to /usr/bin/xray.
Uses its own config path and init script.
endef

define Package/xray-core/conffiles
/etc/xray/
/etc/config/xray
endef

define Package/xray-example
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Example Xray config files
  MAINTAINER:=DOTYWRT <admin@dotycat.com>
  DEPENDS:=xray-core
endef

define Package/xray-example/description
Installs example configuration files for Xray.
endef

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	unzip -o $(DL_DIR)/$(PKG_SOURCE) -d $(PKG_BUILD_DIR)
	chmod +x $(PKG_BUILD_DIR)/xray
	mv $(PKG_BUILD_DIR)/xray $(PKG_BUILD_DIR)/xray25.3.31
endef

define Build/Compile
	# No compile step
endef

define Package/xray-core/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/xray25.3.31 $(1)/usr/bin/xray

	$(INSTALL_DIR) $(1)/etc/xray
	$(INSTALL_DATA) $(CURDIR)/files/config.json.example $(1)/etc/xray/config.json

	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) $(CURDIR)/files/xray.conf $(1)/etc/config/xray

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) $(CURDIR)/files/xray.init $(1)/etc/init.d/xray
endef

define Package/xray-example/install
	$(INSTALL_DIR) $(1)/etc/xray/
	$(INSTALL_DATA) $(CURDIR)/files/vpoint_socks_vmess.json $(1)/etc/xray/
	$(INSTALL_DATA) $(CURDIR)/files/vpoint_vmess_freedom.json $(1)/etc/xray/
endef

$(eval $(call BuildPackage,xray-core))
$(eval $(call BuildPackage,xray-example))
