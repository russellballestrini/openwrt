#
# Copyright (C) 2026 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=usb3-late-load
PKG_RELEASE:=1
PKG_LICENSE:=GPL-2.0-only
PKG_LICENSE_FILES:=
PKG_MAINTAINER:=Russell Ballestrini <russell@unturf.com>

include $(INCLUDE_DIR)/package.mk

define Package/usb3-late-load
  SECTION:=kernel
  CATEGORY:=Kernel modules
  SUBMENU:=USB Support
  TITLE:=Load the xHCI (USB 3.0) modules after preinit
  DEPENDS:=+kmod-usb3
  PKGARCH:=all
endef

define Package/usb3-late-load/description
 Keeps kmod-usb3 and kmod-usb-xhci-hcd out of /etc/modules-boot.d, so the
 xHCI host controller probes from /etc/modules.d in normal boot (after the
 overlay is mounted) instead of in preinit. For boards whose xHCI controller
 hard-locks the CPU when probed a few seconds after a cold power-on; the
 modules still load, and USB 3.0 devices still enumerate at SuperSpeed, a
 few seconds later. A USB 3.0 device cannot serve as extroot with this
 package installed.
endef

define Build/Compile
endef

define Package/usb3-late-load/install
	$(INSTALL_DIR) $(1)/etc/modules-boot.d
endef

define Package/usb3-late-load/postinst
#!/bin/sh
for f in 54-usb3 54-usb-xhci-hcd; do
	rm -f "$${IPKG_INSTROOT}/etc/modules-boot.d/$$f"
done
exit 0
endef

define Package/usb3-late-load/prerm
#!/bin/sh
for f in 54-usb3 54-usb-xhci-hcd; do
	[ -e "$${IPKG_INSTROOT}/etc/modules.d/$$f" ] && ln -sf "../modules.d/$$f" "$${IPKG_INSTROOT}/etc/modules-boot.d/$$f"
done
exit 0
endef

$(eval $(call BuildPackage,usb3-late-load))
