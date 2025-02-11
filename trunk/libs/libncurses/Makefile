SRC_NAME=ncurses-6.5
SRC_URL=http://ftp.gnu.org/pub/gnu/ncurses/ncurses-6.5.tar.gz

all: download_test extract_test config_test
	$(MAKE) -j$(HOST_NCPU) -C $(SRC_NAME)

download_test:
	( if [ ! -f ncurses-6.5.tar.gz ]; then \
		wget -t5 --timeout=20 --no-check-certificate -O ncurses-6.5.tar.gz http://ftp.gnu.org/pub/gnu/ncurses/ncurses-6.5.tar.gz; \
	fi )

extract_test:
	( if [ ! -d ncurses-6.5 ]; then \
		tar xvf ncurses-6.5.tar.gz ; \
		cp -r ncurses-6.5 ncurses-6.5-original ; \
		# 如果需要修改，在这里进行 \
		diff -Naur ncurses-6.5-original ncurses-6.5 > ncurses-6.5.patch ; \
	fi )

config_test:
	( if [ -f ./config_done ]; then \
		echo "the same configuration"; \
	else \
		make configure && touch config_done; \
	fi )

configure:
	( cd $(SRC_NAME) ; \
	./configure \
    --prefix=$(STAGEDIR) \
    --with-install-prefix=$(STAGEDIR) \
    --without-ada \
    --without-termlib \
    --without-progs \
    --without-tests \
    --without-manpages \
    --enable-widec \
    --with-shared \
    --without-debug \
    --with-normal \
    --without-profile \
    --enable-overwrite \
    --enable-pc-files \
    --with-pkg-config-libdir="$(STAGEDIR)/lib/pkgconfig/" \
    --with-ticlib \
    --host=$(HOST_TARGET) \
    --build=$(HOST_BUILD) \
	)

clean:
	if [ -f $(SRC_NAME)/Makefile ] ; then \
		$(MAKE) -C $(SRC_NAME) distclean ; \
	fi ; \
	rm -f config_done

install:
	$(MAKE) -C $(SRC_NAME) install DESTDIR=""

romfs:
	cp -fP $(DESTDIR)/lib/libncursesw.so* $(ROMFSDIR)/lib
