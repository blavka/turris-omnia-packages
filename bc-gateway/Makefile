include $(TOPDIR)/rules.mk

PKG_NAME:=bc-gateway
PKG_VERSION:=0.0.1
PKG_RELEASE:=1
PKG_LICENSE:=MIT

include $(INCLUDE_DIR)/package.mk

define Package/bc-gateway
  SECTION:=utils
  CATEGORY:=Utilities
  SUBMENU:=Home Automation
  TITLE:=bc-gateway
  MAINTAINER:=Karel
  DEPENDS:=+python3 +python3-pyyaml +python3-pyserial +python3-paho-mqtt
endef

define Package/bc-gateway/description
	BigClown gateway between USB and MQTT broker.
endef

define Package/bc-gateway/conffiles
/etc/config/bc-gateway
endef

define Build/Prepare
	$(INSTALL_DIR) $(PKG_BUILD_DIR)
	$(INSTALL_DATA) ./src/bc-gateway.py $(PKG_BUILD_DIR)/
	$(SED) 's/^__version__ = ".*"/__version__ = "$(PKG_VERSION)"/g' $(PKG_BUILD_DIR)/bc-gateway.py
endef

define Build/Compile
endef

define Package/bc-gateway/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bc-gateway.py $(1)/usr/bin/bc-gateway

	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DATA) ./files/bc-gateway.conf $(1)/etc/config/bc-gateway

	$(INSTALL_DIR) $(1)/etc/init.d/
	$(INSTALL_BIN) ./files/bc-gateway.init $(1)/etc/init.d/bc-gateway

endef

define Package/bc-gateway/prerm
#!/bin/sh
set -x
/etc/init.d/bc-gateway disable
/etc/init.d/bc-gateway stop
endef

define Package/bc-gateway/postinst
#!/bin/sh
set -x
/etc/init.d/bc-gateway enable
/etc/init.d/bc-gateway start
endef

$(eval $(call BuildPackage,bc-gateway))
