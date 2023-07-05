THISDIR := $(shell pwd)
NAIVE_TAG := v114.0.5735.91-3
NAIVE_URL_BASE := https://github.com/klzgrad/naiveproxy/releases/download/$(NAIVE_TAG)/

NAIVE_NAME := naiveproxy-$(NAIVE_TAG)-openwrt-mipsel_24kc-static
NAIVE_URL := $(NAIVE_URL_BASE)/$(NAIVE_NAME).tar.xz

CERT_FILE := https://curl.se/ca/cacert.pem

all: download_test extra_test
	@echo "xray build done!"

download_test:
	( if [ ! -f $(NAIVE_NAME).tar.xz ]; then \
		wget -t5 --timeout=20 --no-check-certificate -O $(NAIVE_NAME).tar.xz $(NAIVE_URL); \
		wget -t5 --timeout=20 --no-check-certificate $(CERT_FILE); \
	fi )

extra_test:
	( if [ ! -d $(NAIVE_NAME) ]; then \
		tar xf $(NAIVE_NAME).tar.xz; \
		mv $(NAIVE_NAME)/naive naive; \
	fi )

clean:
	rm -rf $(NAIVE_NAME).tar.xz $(NAIVE_NAME)

romfs:
ifeq ($(CONFIG_FIRMWARE_INCLUDE_XRAY),y)
	$(ROMFSINST) -p +x $(THISDIR)/naive /usr/bin/naive
	$(ROMFSINST) -p -x $(THISDIR)/cacert.pem /usr/lib/cacert.pem
endif
