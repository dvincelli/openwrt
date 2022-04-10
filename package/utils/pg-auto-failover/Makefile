#
# Copyright (C) 2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=pg-auto-failover
PKG_VERSION:=1.4.0
PKG_RELEASE:=1

PKG_SOURCE:=v$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/citusdata/pg_auto_failover/archive/
PKG_HASH:=43cf6a4436cc558772f725d53b910b53d8d7232ef669b8d7942e1318931bb16d
PKG_BUILD_DIR:=$(BUILD_DIR)/pg_auto_failover-$(PKG_VERSION)

PKG_MAINTAINER:=David Vincelli <dvincelli@gmail.com>
PKG_LICENSE:=PostgreSQL
PKG_LICENSE_FILES:=LICENSE

#PKG_USE_MIPS16:=0

include $(INCLUDE_DIR)/package.mk


define Package/pg-auto-failover
  SECTION:=utils
  SUBMENU:=database
  CATEGORY:=Utilities
  TITLE:=PostgreSQL automatic failover
  URL:=https://github.com/citusdata/pg_auto_failover/
  DEPENDS:=+libpq +pgsql-server +pgsql-cli-extra
endef

define Package/pg-auto-failover/description
  pg_auto_failover is an extension and service for PostgreSQL that monitors and manages automated failover for a Postgres cluster.
endef

define Build/Configure
   echo "No-op"
endef

define Build/Compile
	# The PATH var is required so that pg-auto-failover finds pg_config
	$(call Build/Compile/Default, \
		install , \
		PATH=$(STAGING_DIR)/usr/bin:$(PATH) \
	)
endef

define Package/pg-auto-failover/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/bin/pg_autoctl/pg_autoctl $(1)/usr/bin

	$(INSTALL_DIR) $(1)/usr/share/postgresql/extension/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/monitor/pgautofailover.control $(1)/usr/share/postgresql/extension
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/monitor/*.sql $(1)/usr/share/postgresql/extension

	$(INSTALL_DIR) $(1)/usr/lib/postgresql
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/monitor/pgautofailover.so $(1)/usr/lib/postgresql
endef

$(eval $(call BuildPackage,pg-auto-failover))
