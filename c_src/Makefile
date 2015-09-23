LINUX=$(shell uname | grep Linux | wc -l | xargs echo)
DEPS=../deps

ifeq ($(LINUX),1)
ZMQ_FLAGS=--with-pic --with-pgm --enable-static=yes
else
ZMQ_FLAGS=
endif

ifndef ZEROMQ_VERSION
ZEROMQ_VERSION=4.0.3
endif

ifndef LIBSODIUM_VERSION
LIBSODIUM_VERSION=1.0.3
endif

all: $(DEPS)/zeromq4/src/.libs/libzmq.a $(DEPS)/libsodium/src/libsodium/.libs/libsodium.a

clean:
	if test -e $(DEPS)/zeromq4/Makefile; then \
		cd $(DEPS)/zeromq4; make clean; \
	else \
		true; \
	fi

distclean:
	@rm -rf $(DEPS)

$(DEPS)/zeromq4:
	@mkdir -p $(DEPS)
	@curl http://download.zeromq.org/zeromq-$(ZEROMQ_VERSION).tar.gz -o $(DEPS)/zeromq-$(ZEROMQ_VERSION).tar.gz
	@cd $(DEPS) && tar xzvfp zeromq-$(ZEROMQ_VERSION).tar.gz && mv zeromq-$(ZEROMQ_VERSION) zeromq4

$(DEPS)/zeromq4/src/.libs/libzmq.a: $(DEPS)/zeromq4
	@cd $(DEPS)/zeromq4 && ./configure $(ZMQ_FLAGS) && make

$(DEPS)/libsodium:
	@mkdir -p $(DEPS)
	curl -L https://github.com/jedisct1/libsodium/releases/download/$(LIBSODIUM_VERSION)/libsodium-$(LIBSODIUM_VERSION).tar.gz -o $(DEPS)/libsodium-$(LIBSODIUM_VERSION).tar.gz
	@cd $(DEPS) && tar xzvfp libsodium-$(LIBSODIUM_VERSION).tar.gz && mv libsodium-$(LIBSODIUM_VERSION) libsodium

$(DEPS)/libsodium/src/libsodium/.libs/libsodium.a: $(DEPS)/libsodium
	@cd $(DEPS)/libsodium && ./configure --with-pic && make
