#
#       Copyright (C) 1997 Claus-Justus Heine.
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
# $Source: /home/cvsroot/RT2880-Linux/src/linux-2.4.30/drivers/char/ftape/compressor/Makefile,v $
# $Revision: #1 $
# $Date: 2014/05/07 $
#
#      Makefile for the optional compressor for th zftape VFS
#      interface to the QIC-40/80/3010/3020 floppy-tape driver for
#      Linux.
#

O_TARGET := zft-compressor.o

obj-y    = zftape-compress.o lzrw3.o

obj-m    = $(O_TARGET)

CFLAGS_lzrw3.o	:= -O6 -funroll-all-loops

include $(TOPDIR)/Rules.make
