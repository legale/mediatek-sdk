# /**************************************
#  ecmh - Easy Cast du Multi Hub
#  by Jeroen Massar <jeroen@unfix.org>
# ***************************************
# $Author: red.hung $
# $Id: //WIFI_SOC/MP/SDK_4_3_0_0/RT288x_SDK/source/user/ecmh/tools/mtrace6/Makefile#1 $
# $Date: 2014/05/07 $
# **************************************/
#
# Tools Makefile for ecmh - Jeroen Massar <jeroen@unfix.org>
#
# mtrace6 is taken from the KAME distribution, see http://www.kame.net
# Copyright etc is theirs, it is only included for convienience.

BINS	= mtrace6
SRCS	= mtrace6.c
INCS	= ../../src/trace.h
DEPS	= ../../Makefile ../Makefile Makefile
OBJS	= mtrace6.o
CFLAGS	= -W -Wall -Wno-unused -D_GNU_SOURCE -D'ECMH_VERSION="$(ECMH_VERSION)"' $(ECMH_OPTIONS)
LDFLAGS	= 
CC      = gcc
RM      = rm

all:	$(BINS)

mtrace6: $(OBJS) ${INCS} ${DEPS}
	$(CC) $(CFLAGS) -o $@ $(OBJS) $(LDFLAGS);
	strip $@

clean:
	$(RM) -f $(OBJS) $(BINS)

# Mark targets as phony
.PHONY : all clean mtrace6

