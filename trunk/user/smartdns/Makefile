SRC_NAME = smartdns-Release35
THISDIR = $(shell pwd)
XRAY_URL_BASE := https://github.com/CTZZG/xray-openwrt/releases/latest/download/

XRAY_NAME := xray-linux-mipsle
XRAY_URL := $(XRAY_URL_BASE)/$(XRAY_NAME).tar.gz

CERT_FILE := https://curl.se/ca/cacert.pem
GEOIP_FILE := https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geoip.dat
GEOSITE_FILE := https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geosite.dat

all: extract_test config_test download_test extra_test
	$(MAKE) -C $(SRC_NAME)

extract_test:
	( if [ ! -d $(SRC_NAME) ]; then \
		tar zxf $(SRC_NAME).tar.gz ; \
	fi )

config_test:
	( if [ -f ./config_done ]; then \
		echo "the same configuration"; \
	else \
		touch config_done; \
	fi )

download_test:
	( if [ ! -f $(XRAY_NAME).tar.gz ]; then \
		wget -t5 --timeout=20 --no-check-certificate -O $(XRAY_NAME).tar.gz $(XRAY_URL); \
		wget -t5 --timeout=20 --no-check-certificate $(CERT_FILE); \
		wget -t5 --timeout=20 --no-check-certificate $(GEOIP_FILE); \
		wget -t5 --timeout=20 --no-check-certificate $(GEOSITE_FILE); \
	fi )

extra_test:
	( if [ ! -d $(XRAY_NAME) ]; then \
		tar xf $(XRAY_NAME).tar.gz; \
		rm xray; \
		mv xray_softfloat xray; \
	fi )

clean:
	if [ -f $(SRC_NAME)/Makefile ] ; then \
		$(MAKE) -C $(SRC_NAME) clean ; \
	fi ; \
	rm -f config_done
	rm -rf $(XRAY_NAME).tar.gz

romfs:
	$(ROMFSINST) -p +x $(THISDIR)/$(SRC_NAME)/src/smartdns /usr/bin/smartdns
	$(ROMFSINST) -p +x $(THISDIR)/smartdns.sh /usr/bin/smartdns.sh
	$(ROMFSINST) /etc_ro/smartdns_address.conf
	$(ROMFSINST) /etc_ro/smartdns_blacklist-ip.conf
	$(ROMFSINST) /etc_ro/smartdns_custom.conf
	$(ROMFSINST) /etc_ro/smartdns_whitelist-ip.conf
ifeq ($(CONFIG_FIRMWARE_INCLUDE_XRAY),y)
	$(ROMFSINST) -p +x $(THISDIR)/xray /usr/bin/xray
	$(ROMFSINST) -p +x $(THISDIR)/geoip.dat /usr/bin/geoip.dat
	$(ROMFSINST) -p +x $(THISDIR)/geosite.dat /usr/bin/geosite.dat
	$(ROMFSINST) -p -x $(THISDIR)/cacert.pem /usr/lib/cacert.pem
endif
