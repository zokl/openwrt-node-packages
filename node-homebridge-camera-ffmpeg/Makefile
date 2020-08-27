# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NPM_NAME:=homebridge-camera-ffmpeg
PKG_NAME:=node-$(PKG_NPM_NAME)
PKG_VERSION:=3.0.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NPM_NAME)-$(PKG_VERSION).tgz
PKG_SOURCE_URL:=https://registry.npmjs.org/$(PKG_NPM_NAME)/-/
PKG_HASH:=8901acba1d089eba75b364fce0ac655986f9849ecee191c65754fb104c425c2c

PKG_BUILD_DEPENDS:=node/host
HOST_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

PKG_MAINTAINER:=Zbynek Kocur <zokl@atlas.cz>
PKG_LICENSE:=Apache
PKG_LICENSE_FILES:=LICENSE

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
  SUBMENU:=Node.js
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=homebridge camera ffmpeg
  URL:=https://www.npmjs.org/package/homebridge-camera-ffmpeg
  DEPENDS:= \
	+node \
	+node-homebridge \
	+libffmpeg-custom \
	+@FFMPEG_CUSTOM_GPL\
	+@FFMPEG_CUSTOM_GPLV3 \
	+@FFMPEG_CUSTOM_NONFREE \
	+@FFMPEG_CUSTOM_PATENTED \
	+@FFMPEG_CUSTOM_LARGE \
	+@FFMPEG_CUSTOM_PROGRAMS \
	+@FFMPEG_CUSTOM_ENCODER_jpegls\
	+@FFMPEG_CUSTOM_ENCODER_png \
	+@FFMPEG_CUSTOM_SELECT_libfdk-aac \
	+@FFMPEG_CUSTOM_SELECT_libmp3lame \
	+@FFMPEG_CUSTOM_SELECT_libopus \
	+@FFMPEG_CUSTOM_SELECT_libx264 \
	+@FFMPEG_CUSTOM_DECODER_hevc \
	+@FFMPEG_CUSTOM_DECODER_jpegls \
	+@FFMPEG_CUSTOM_DECODER_png \
	+@FFMPEG_CUSTOM_DECODER_amrnb \
	+@FFMPEG_CUSTOM_DECODER_amrwb \
	+@FFMPEG_CUSTOM_DEMUXER_amr \
	+@FFMPEG_CUSTOM_DEMUXER_hevc \
	+@FFMPEG_CUSTOM_DEMUXER_rtsp \
	+@FFMPEG_CUSTOM_PARSER_h264 \
	+@FFMPEG_CUSTOM_PROTOCOL_file \
	+@FFMPEG_CUSTOM_PROTOCOL_http \
	+@FFMPEG_CUSTOM_PROTOCOL_pipe \
	+@FFMPEG_CUSTOM_PROTOCOL_rtp \
	+@FFMPEG_CUSTOM_PROTOCOL_tcp
endef

define Package/$(PKG_NAME)/description
 Homebridge Plugin Providing FFmpeg-based Camera Support.
endef

TAR_OPTIONS+= --strip-components 1
TAR_CMD=$(HOST_TAR) -C $(1) $(TAR_OPTIONS)

NODEJS_CPU:=$(subst powerpc,ppc,$(subst aarch64,arm64,$(subst x86_64,x64,$(subst i386,ia32,$(ARCH)))))
TMPNPM:=$(shell mktemp -u XXXXXXXXXX)

define Build/Compile
	$(MAKE_VARS) \
	$(MAKE_FLAGS) \
	npm_config_arch=$(NODEJS_CPU) \
	npm_config_target_arch=$(NODEJS_CPU) \
	npm_config_build_from_source=true \
	npm_config_nodedir=$(STAGING_DIR)/usr/ \
	npm_config_prefix=$(PKG_INSTALL_DIR)/usr/ \
	npm_config_cache=$(TMP_DIR)/npm-cache-$(TMPNPM) \
	npm_config_tmp=$(TMP_DIR)/npm-tmp-$(TMPNPM) \
	npm install -g $(PKG_BUILD_DIR)
	rm -rf $(TMP_DIR)/npm-tmp-$(TMPNPM)
	rm -rf $(TMP_DIR)/npm-cache-$(TMPNPM)
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/lib/node_modules/$(PKG_NPM_NAME)
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/$(PKG_NPM_NAME)/{*.json,LICENSE,*.md} \
                $(1)/usr/lib/node_modules/$(PKG_NPM_NAME)
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/$(PKG_NPM_NAME)/{dist,node_modules} \
                $(1)/usr/lib/node_modules/$(PKG_NPM_NAME)
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
