THISDIR = $(shell pwd)
v2ray_dir="v2ray.com/core/main"
export GO111MODULE=on
export GOPROXY=https://goproxy.cn
V2ray_VERSION := 5.12.1
V2ray_URL := https://codeload.github.com/v2fly/v2ray-core/tar.gz/v$(V2ray_VERSION)
V2RAY_SED_ARGS:=
V2RAY_SED_ARGS += \
	s/_ "v2ray.com\/core\/main\/json"/\/\/ &/; \
	/\/\/ _ "v2ray.com\/core\/main\/jsonem"/s/\/\/ //;
V2RAY_SED_ARGS += \
	s/_ "v2ray.com\/core\/main\/json"/\/\/ &/;

all:download_v2 build_extract build_v2ray
#all:download_v2 

download_v2:
	( if [ ! -f $(THISDIR)/v2ray-core-$(V2ray_VERSION).tar.gz ]; then \
	curl --create-dirs -L $(V2ray_URL) -o $(THISDIR)/v2ray-core-$(V2ray_VERSION).tar.gz ; \
	fi )

build_extract:
	mkdir -p $(THISDIR)/v2ray.com
	mkdir -p $(THISDIR)/bin
	( if [ ! -d $(THISDIR)/v2ray.com/core ]; then \
	tar zxfv $(THISDIR)/v2ray-core-$(V2ray_VERSION).tar.gz -C $(THISDIR)/v2ray.com ; \
	mv $(THISDIR)/v2ray.com/v2ray-core-$(V2ray_VERSION) $(THISDIR)/v2ray.com/core ; \
	fi )

build_v2ray:
	( cd $(THISDIR)/$(v2ray_dir); \
	sed -i \
			'$(V2RAY_SED_ARGS)' \
			distro/all/all.go ; \
	GOOS=linux GOARCH=mipsle go build -ldflags "-w -s" -o $(THISDIR)/bin/v2ray; \
	)

clean:
	#rm -rf $(THISDIR)/v2ray.com
	#rm -rf $(THISDIR)/bin

romfs:
	$(ROMFSINST) -p +x $(THISDIR)/bin/v2ray /usr/bin/v2ray
