#
# pppd makefile for Linux
# $Id: //WIFI_SOC/MP/SDK_4_3_0_0/RT288x_SDK/source/user/ppp-2.4.2/pppd/Makefile#1 $
#
TARGETS = pppd

PPPDSRCS = main.c magic.c fsm.c lcp.c ipcp.c upap.c chap-new.c md5.c ccp.c \
	   ecp.c ipxcp.c auth.c options.c sys-linux.c md4.c chap_ms.c \
	   demand.c utils.c tty.c eap.c chap-md5.c

HEADERS = ccp.h chap-new.h ecp.h fsm.h ipcp.h \
	ipxcp.h lcp.h magic.h md5.h patchlevel.h pathnames.h pppd.h \
	upap.h eap.h

MANPAGES = pppd.8
PPPDOBJS = main.o magic.o fsm.o lcp.o ipcp.o upap.o chap-new.o md5.o ccp.o \
	   ecp.o auth.o options.o demand.o utils.o sys-linux.o ipxcp.o tty.o \
	   eap.o chap-md5.o

#
# include dependencies if present
ifeq (.depend,$(wildcard .depend))
include .depend
endif

COPTS = -O2 -pipe -Wall -g
LIBS =

# Uncomment the next 2 lines to include support for Microsoft's
# MS-CHAP authentication protocol.  Also, edit plugins/radius/Makefile.linux.
CHAPMS=y
USE_CRYPT=y
# Don't use MSLANMAN unless you really know what you're doing.
#MSLANMAN=y
# Uncomment the next line to include support for MPPE.  CHAPMS (above) must
# also be enabled.  Also, edit plugins/radius/Makefile.linux.
MPPE=y

# Uncomment the next line to include support for PPP packet filtering.
# This requires that the libpcap library and headers be installed
# and that the kernel driver support PPP packet filtering.
#FILTER=y

# Uncomment the next line to enable multilink PPP (enabled by default)
# Linux distributions: Please leave multilink ENABLED in your builds
# of pppd!
HAVE_MULTILINK=y

# Uncomment the next line to enable the TDB database (enabled by default.)
# If you enable multilink, then TDB is automatically enabled also.
# Linux distributions: Please leave TDB ENABLED in your builds.
#USE_TDB=y

#HAS_SHADOW=y
#USE_PAM=y
HAVE_INET6=y

# Enable plugins
PLUGIN=y

# Enable Microsoft proprietary Callback Control Protocol
#CBCP=y

# Enable EAP SRP-SHA1 authentication (requires libsrp)
#USE_SRP=y

#MAXOCTETS=y

INCLUDE_DIRS= -I../include

COMPILE_FLAGS= #-DHAVE_PATHS_H -DIPX_CHANGE -DHAVE_MMAP

CFLAGS= $(COPTS) $(COMPILE_FLAGS) $(INCLUDE_DIRS)

ifdef CHAPMS
CFLAGS   += -DCHAPMS=1
NEEDDES=y
PPPDOBJS += md4.o chap_ms.o
HEADERS	+= md4.h chap_ms.h
ifdef MSLANMAN
CFLAGS   += -DMSLANMAN=1
endif
ifdef MPPE
CFLAGS   += -DMPPE=1
endif
endif

# EAP SRP-SHA1
ifdef USE_SRP
CFLAGS	+= -DUSE_SRP -DOPENSSL -I/usr/local/ssl/include
LIBS	+= -lsrp -L$(ROOTDIR)/lib/lib -lcrypto
TARGETS	+= srp-entry
EXTRAINSTALL = $(INSTALL) -s -c -m 555 srp-entry $(BINDIR)/srp-entry
MANPAGES += srp-entry.8
EXTRACLEAN += srp-entry.o
NEEDDES=y
else
# OpenSSL has an integrated version of SHA-1, and its implementation
# is incompatible with this local SHA-1 implementation.  We must use
# one or the other, not both.
PPPDSRCS += sha1.c
HEADERS += sha1.h
PPPDOBJS += sha1.o
endif

ifdef HAS_SHADOW
CFLAGS   += -DHAS_SHADOW
#LIBS     += -lshadow $(LIBS)
endif

ifneq ($(wildcard $(ROOTDIR)/lib/include/crypt.h),)
CFLAGS   += -DHAVE_CRYPT_H=1
endif
ifneq ($(wildcard $(ROOTDIR)/lib/lib/libcrypt.*),)
LIBS	+= -lcrypt
endif
ifneq ($(wildcard $(ROOTDIR)/uClibc-0.9.33.2/lib/libcrypt.*),)
LIBS	+= -lcrypt
endif

ifdef NEEDDES
ifndef USE_CRYPT
LIBS     += -ldes $(LIBS)
else
CFLAGS   += -DUSE_CRYPT=1
endif
PPPDOBJS += pppcrypt.o
HEADERS += pppcrypt.h
endif

# For "Pluggable Authentication Modules", see ftp.redhat.com:/pub/pam/.
ifdef USE_PAM
CFLAGS   += -DUSE_PAM
LIBS     += -lpam -ldl
endif

# Multi-linnk
ifdef HAVE_MULTILINK
	# Multilink implies the use of TDB
	USE_TDB=y

	CFLAGS += -DHAVE_MULTILINK
	PPPDSRCS += multilink.c
	PPPDOBJS += multilink.o
endif

# TDB
ifdef USE_TDB
	CFLAGS += -DUSE_TDB=1
	PPPDSRCS += tdb.c
	PPPDOBJS += tdb.o
	HEADERS += tdb.h
endif

# Lock library binary for Linux is included in 'linux' subdirectory.
ifdef LOCKLIB
LIBS     += -llock
CFLAGS   += -DLOCKLIB=1
endif

ifdef PLUGIN
CFLAGS	+= -DPLUGIN
LDFLAGS	+= -Wl,-E
LIBS	+= -ldl
endif

ifdef FILTER
LIBS    += -lpcap
CFLAGS  += -DPPP_FILTER -I/usr/include/pcap
endif

ifdef HAVE_INET6
     PPPDSRCS += ipv6cp.c eui64.c
     HEADERS  += ipv6cp.h eui64.h
     PPPDOBJS += ipv6cp.o eui64.o
     CFLAGS   += -DINET6=1
endif

ifdef CBCP
     PPPDSRCS += cbcp.c
     PPPDOBJS += cbcp.o
     CFLAGS += -DCBCP_SUPPORT
     HEADERS += cbcp.h
endif

ifdef MAXOCTETS
     CFLAGS += -DMAXOCTETS
endif

INSTALL= install -o root

all: $(TARGETS)

$(TARGETS): $(PPPDOBJS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o pppd $(PPPDOBJS) $(LIBS)

srp-entry:	srp-entry.c
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ srp-entry.c $(LIBS)

romfs: $(TARGETS)
	$(ROMFSINST) /bin/$(TARGETS)
	$(ROMFSINST) /etc_ro/ppp/ip-up
	ln -sf /etc/3g $(ROMFSDIR)/etc_ro/ppp/peers/3g 

clean:
	rm -f $(PPPDOBJS) $(EXTRACLEAN) $(TARGETS) *~ #* core

depend:
	$(CPP) -M $(CFLAGS) $(PPPDSRCS) >.depend
