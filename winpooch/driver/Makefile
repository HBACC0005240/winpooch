##################################################################
#                                                                #
#  Winpooch : Windows Watchdog                                   #
#  Copyright (C) 2004-2005  Benoit Blanchon                      #
#                                                                #
#  This program is free software; you can redistribute it        #
#  and/or modify it under the terms of the GNU General Public    #
#  License as published by the Free Software Foundation; either  #
#  version 2 of the License, or (at your option) any later       #
#  version.                                                      #
#                                                                #
#  This program is distributed in the hope that it will be       #
#  useful, but WITHOUT ANY WARRANTY; without even the implied    #
#  warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR       #
#  PURPOSE.  See the GNU General Public License for more         #
#  details.                                                      #
#                                                                #
#  You should have received a copy of the GNU General Public     #
#  License along with this program; if not, write to the Free    #
#  Software Foundation, Inc.,                                    #
#  675 Mass Ave, Cambridge, MA 02139, USA.                       #
#                                                                #
##################################################################

DEFINES = -DDBG -D__NTDDK__ # -DMALLOC_STATISTICS
INCLUDES = -I../common
CFLAGS = -Wall -Werror -gcoff $(DEFINES) $(INCLUDES)
LDLIBS =


#
# Programs needed
#
CC	= gcc
LD	= gcc -shared -Wl,--entry,_DriverEntry@8 
MKDEP	= gcc -MM $(CFLAGS)
MV	= mv -f
RC	= windres $(DEFINES)
RM	= rm -f
SED	= sed
STRIP	= strip

#
# Driver specific stuffs
#
DRIVER = Winpooch
DRIVER_BIN = $(DRIVER).sys
DRIVER_LDLIBS = -lntoskrnl -nostartfiles -nostdlib -lhal
DRIVER_SRCS = \
	../common/FiltRule.c \
	../common/FiltCond.c \
	../common/FiltParam.c \
	../common/FilterSet.c \
	../common/Filter.c \
	../common/Malloc.c \
	../common/ImgInfo.c \
	../common/Strlcpy.c \
	../common/Wildcards.c \
	Driver.c \
	DrvFilter.c \
	DrvStatus.c \
	Disasm.c \
	FileInfo.c \
	Hook.c \
	HookCommon.c \
	HookFile.c \
	HookReg.c \
	Hooks.c \
	HookSys.c \
	Link.c \
	NtUndoc.c \
	ProcInfo.c \
	ProcList.c \
	ScanCache.c \
	SystInfo.c \
	WatchedObjects.c
DRIVER_OBJS = $(patsubst %.c,%.ko,$(DRIVER_SRCS))


all : $(DRIVER_BIN)

clean :
	$(RM) *.ko ../common/*.ko *.a *.d *.c~ *.h~ *.rc~ *.pl~ Makefile~

strip : $(DRIVER_BIN)
	$(STRIP) $(DRIVER_BIN)

BuildCount.h : *.c *.h ../common/*.c ../common/*.h
	perl ../common/BuildCounter.pl $@ DRIVER_BUILD

$(DRIVER_BIN) : $(DRIVER_OBJS)
	$(LD) $(DRIVER_LDFLAGS) $^ $(DRIVER_LDLIBS) -o $@

%.ko : %.c
	$(CC) $(CFLAGS) -c -o $@ $<

%.ko : %.rc
	$(RC) $(INCLUDES) -o $@ $<

%.d : %.c
	$(MKDEP) $< > $@.$$$$; \
	$(SED) 's,\($*\)\.o[ :]*,\1.ko $@ : ,g'  $@.$$$$ > $@; \
	$(RM) $@.$$$$

#
# Automatic dependencies
#
-include $(DRIVER_SRCS:.c=.d)
