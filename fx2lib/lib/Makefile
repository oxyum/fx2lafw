# Copyright (C) 2009 Ubixum, Inc. 
# 
# This library is free software; you can redistribute it and/or
# modify it under the terms of the GNU Lesser General Public
# License as published by the Free Software Foundation; either
# version 2.1 of the License, or (at your option) any later version.
# 
# This library is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
# Lesser General Public License for more details.
# 
# You should have received a copy of the GNU Lesser General Public
# License along with this library; if not, write to the Free Software
# Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA  02110-1301  USA

SOURCES = serial.c i2c.c delay.c setupdat.c gpif.c eputils.c $(wildcard interrupts/*.c)
FX2_OBJS = $(patsubst %.c,%.rel, $(SOURCES)) usbav.rel int4av.rel
INCLUDES = -I../include
SDCC = sdcc -mmcs51 $(SDCCFLAGS)
LIBS = fx2.lib

# The sdcc 8051 assembler binary has/had different names.
ifneq ($(shell which sdas8051 2>/dev/null),)
# Newer sdcc, Debian / Ubuntu
SDAS8051 = sdas8051
endif
ifneq ($(shell which sdcc-sdas8051 2>/dev/null),)
# Newer sdcc, Fedora
SDAS8051 = sdcc-sdas8051
endif
ifneq ($(shell which asx8051 2>/dev/null),)
# Older sdcc
SDAS8051 = asx8051
endif

all: $(LIBS)

$(LIBS): $(FX2_OBJS)
	sdcclib fx2.lib $?

usbav.rel: usbav.a51
	$(SDAS8051) -logs usbav.a51

int4av.rel: int4av.a51
	$(SDAS8051) -logs int4av.a51

%.rel: %.c
	$(SDCC) $(INCLUDES) -c $< -o $@

clean:
	rm -f *.asm *.ihx *.lnk *.lst *.map *.mem *.rel *.rst *.sym
	rm -f *.adb *.cdb *.lib
	rm -f interrupts/*.asm interrupts/*.ihx interrupts/*.lnk
	rm -f interrupts/*.lst interrupts/*.map interrupts/*.mem
	rm -f interrupts/*.rel interrupts/*.rst interrupts/*.sym
	rm -f interrupts/*.adb interrupts/*.dcb interrupts/*.lib

