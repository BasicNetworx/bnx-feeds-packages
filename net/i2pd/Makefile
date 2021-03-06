#
# Copyright (C) 2015, 2016 gxcreator
# Copyright (C) 2017 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=i2pd
PKG_VERSION:=2.30.0
PKG_RELEASE:=2
PKG_BUILD_PARALLEL:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/PurpleI2P/i2pd/tar.gz/$(PKG_VERSION)?
PKG_HASH:=25915cbd33a9f53c89ddf7fbd68fccc5ffc89ab40d4445ccc813da74fae154f2

PKG_MAINTAINER:=David Yang <mmyangfl@gmail.com>
PKG_LICENSE:=BSD-3-Clause
PKG_LICENSE_FILES:=LICENSE

include $(INCLUDE_DIR)/package.mk

define Package/i2pd
	SECTION:=net
	CATEGORY:=Network
	DEPENDS:=+libopenssl +boost +boost-system +boost-filesystem \
	         +boost-program_options +boost-date_time +libatomic +zlib
	TITLE:=full-featured C++ implementation of I2P client
	URL:=https://github.com/PurpleI2P/i2pd
	USERID:=i2pd:i2pd
endef

define Package/i2pd/description
	I2P (Invisible Internet Protocol) is a universal anonymous network layer. All
	communications over I2P are anonymous and end-to-end encrypted, participants
	don't reveal their real IP addresses.
endef

define Package/i2pd/conffiles
/etc/config/i2pd
/etc/i2pd/i2pd.conf
/etc/i2pd/tunnels.conf
/etc/i2pd/tunnels.d/
endef

TARGET_LDFLAGS+=-latomic
MAKE_FLAGS+=USE_AESNI=no USE_AVX=no

define Package/i2pd/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/i2pd $(1)/usr/sbin
	$(INSTALL_DIR) $(1)/usr/share/i2pd
	$(CP) $(PKG_BUILD_DIR)/contrib/certificates  $(1)/usr/share/i2pd
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DATA) ./files/i2pd.config $(1)/etc/config/i2pd
	$(INSTALL_DIR) $(1)/etc/i2pd
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/contrib/i2pd.conf $(1)/etc/i2pd
	$(SED) ' \
		s/127.0.0.1/192.168.1.1/g; \
		s/datadir = \/var\/lib/datadir = \/etc/ \
	' $(1)/etc/i2pd/i2pd.conf
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/contrib/tunnels.conf $(1)/etc/i2pd
	$(INSTALL_DIR) $(1)/etc/i2pd/tunnels.d
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/contrib/tunnels.d/*.conf  $(1)/etc/i2pd/tunnels.d
	# subscriptions.txt deprecated, see #8088
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/i2pd.init $(1)/etc/init.d/i2pd
endef

$(eval $(call BuildPackage,i2pd))
