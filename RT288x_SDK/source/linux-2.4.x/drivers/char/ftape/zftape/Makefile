#
#       Copyright (C) 1996, 1997 Claus-Justus Heine.
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2, or (at your option)
# any later version.
# 
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
# 
# You should have received a copy of the GNU General Public License
# along with this program; see the file COPYING.  If not, write to
# the Free Software Foundation, 675 Mass Ave, Cambridge, MA 02139, USA.
#
# $Source: /home/cvsroot/RT2880-Linux/src/linux-2.4.30/drivers/char/ftape/zftape/Makefile,v $
# $Revision: #1 $
# $Date: 2014/05/07 $
#
#      Makefile for the QIC-40/80/3010/3020 zftape interface VFS to
#      ftape
#


# ZFT_OBSOLETE - enable the MTIOC_ZFTAPE_GETBLKSZ ioctl. You should
#                leave this enabled for compatibility with taper.

EXTRA_CFLAGS := -DZFT_OBSOLETE

O_TARGET := zftape.o

export-objs := zftape_syms.o

obj-y    := zftape-rw.o zftape-ctl.o zftape-read.o \
	    zftape-write.o zftape-vtbl.o zftape-eof.o \
	    zftape-init.o zftape-buffers.o zftape_syms.o

obj-m    := $(O_TARGET)

include $(TOPDIR)/Rules.make

