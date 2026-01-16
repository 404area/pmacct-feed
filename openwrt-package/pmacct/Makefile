include $(TOPDIR)/rules.mk

PKG_NAME:=pmacct
PKG_VERSION:=1.7.9
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://www.pmacct.net/
PKG_HASH:=94185c33d659772035c63be0a82601bb9eb064fb4be1d68ba279b47204b2750a

PKG_MAINTAINER:=YourName
PKG_LICENSE:=GPL-2.0-or-later

PKG_BUILD_DEPENDS:=zlib libpcap libpthread
PKG_BUILD_PARALLEL:=1
PKG_FIXUP:=autoreconf

TARGET_CFLAGS += -Wno-error
TARGET_CXXFLAGS += -Wno-error

include $(INCLUDE_DIR)/package.mk

define Package/pmacct
  SECTION:=net
  CATEGORY:=Network
  TITLE:=pmacct - passive network monitoring (NetFlow/sFlow/IPFIX/BGP...)
  URL:=https://github.com/pmacct/pmacct
  DEPENDS:=+libpcap +zlib +libpthread +librt \
           +PACKAGE_pmacct-with-nfacctd:libmnl \
           +PACKAGE_pmacct-with-uacctd:libcap
endef

define Package/pmacct/description
  Collection and export of network accounting data using NetFlow/sFlow/IPFIX etc.
endef

define Build/Configure
	$(call Build/Configure/Default, \
		--enable-ipv6 \
		--enable-nfacct \
		--enable-uacctd \
		--enable-ulog \
		--disable-debug \
	)
endef

define Package/pmacct/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/nfacctd $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/sfacctd $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/uacctd $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/pmacct $(1)/usr/bin/
	# 如果需要 BGP 等，额外加：
	# $(INSTALL_BIN) $(PKG_BUILD_DIR)/src/pmbgpd $(1)/usr/bin/
endef

$(eval $(call BuildPackage,pmacct))
