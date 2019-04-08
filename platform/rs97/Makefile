# PokeMini Makefile for Dingux
CHAINPREFIX=/opt/mipsel-linux-uclibc
CROSS_COMPILE=$(CHAINPREFIX)/usr/bin/mipsel-linux-

CC = $(CROSS_COMPILE)gcc
CXX = $(CROSS_COMPILE)g++
LD = $(CROSS_COMPILE)gcc
STRIP = $(CROSS_COMPILE)strip

SYSROOT     := $(CHAINPREFIX)/usr/mipsel-buildroot-linux-uclibc/sysroot
SDL_CFLAGS  := $(shell $(SYSROOT)/usr/bin/sdl-config --cflags)
SDL_LIBS    := $(shell $(SYSROOT)/usr/bin/sdl-config --libs)

BUILD = Build
TARGET = pokemini/pokemini.dge
POKEROOT = ../../

INCLUDE = $(foreach inc, $(INCDIRS), -I$(POKEROOT)$(inc))

CFLAGS = -Ofast -mplt -mips32 -Wall -s -lSDL $(SDL_CFLAGS) $(INCLUDE) -DPERFORMANCE -DNO_SCANDIRS -DNO_ZIP
LDFLAGS = -Ofast -mips32 -lm -lz -s -lSDL $(SDL_LIBS)

INCDIRS = source resource freebios dependencies/minizip

OBJS = \
 PokeMini_Dingux.o	\
 freebios/freebios.o	\
 source/PMCommon.o	\
 source/PokeMini.o	\
 source/Multicart.o	\
 source/Hardware.o	\
 source/Video.o	\
 source/Video_x3.o	\
 source/CommandLine.o	\
 source/MinxCPU.o	\
 source/MinxCPU_XX.o	\
 source/MinxCPU_CE.o	\
 source/MinxCPU_CF.o	\
 source/MinxCPU_SP.o \
 source/MinxTimers.o	\
 source/MinxIO.o	\
 source/MinxIRQ.o	\
 source/MinxPRC.o	\
 source/MinxColorPRC.o	\
 source/MinxLCD.o	\
 source/MinxAudio.o	\
 source/UI.o	\
 source/Joystick.o	\
 resource/PokeMini_ColorPal.o	\
 resource/PokeMini_Font12.o	\
 resource/PokeMini_Icons12.o	\
 resource/PokeMini_BG3.o

DEPENDS = \
 freebios/freebios.h	\
 source/IOMap.h	\
 source/PMCommon.h	\
 source/PokeMini.h	\
 source/PokeMini_Version.h	\
 source/Multicart.h	\
 source/Hardware.h	\
 source/Video.h	\
 source/Video_x3.h	\
 source/CommandLine.h	\
 source/MinxCPU.h	\
 source/MinxTimers.h	\
 source/MinxIO.h	\
 source/MinxIRQ.h	\
 source/MinxPRC.h	\
 source/MinxColorPRC.h	\
 source/MinxLCD.h	\
 source/MinxAudio.h	\
 source/UI.h	\
 source/Joystick.h	\
 resource/PokeMini_ColorPal.h	\
 resource/PokeMini_Font12.h	\
 resource/PokeMini_Icons12.h	\
 resource/PokeMini_BG3.h

BUILDOBJS = $(addprefix $(BUILD)/, $(notdir $(OBJS)))
DEPENDSHDR = $(addprefix $(POKEROOT), $(DEPENDS))
VPATH = $(addprefix $(POKEROOT),$(INCDIRS))

.PHONY: all clean

all: $(BUILD) $(TARGET)

$(BUILD):
	@[ -d @ ] || mkdir -p $@

$(BUILD)/%.o: %.c $(DEPENDSHDR)
	$(CC) $(CFLAGS) -o $@ -c $<

$(TARGET): $(BUILDOBJS)
	$(LD) -o $(TARGET) $(BUILDOBJS) $(LDFLAGS)
	$(STRIP) $(TARGET)

ipk:  $(BUILD) $(TARGET)
	@rm -rf /tmp/.pokemini-ipk/ && mkdir -p /tmp/.pokemini-ipk/root/home/retrofw/emus/pokemini /tmp/.pokemini-ipk/root/home/retrofw/apps/gmenu2x/sections/emulators /tmp/.pokemini-ipk/root/home/retrofw/apps/gmenu2x/sections/emulators.systems
	@cp -r pokemini/pokemini.dge pokemini/pokemini.man.txt pokemini/pokemini.png /tmp/.pokemini-ipk/root/home/retrofw/emus/pokemini
	@cp pokemini/pokemini.lnk /tmp/.pokemini-ipk/root/home/retrofw/apps/gmenu2x/sections/emulators
	@sed "s/^Version:.*/Version: $$(date +%Y%m%d)/" pokemini/control > /tmp/.pokemini-ipk/control
	@cp pokemini/conffiles /tmp/.pokemini-ipk/
	@tar --owner=0 --group=0 -czvf /tmp/.pokemini-ipk/control.tar.gz -C /tmp/.pokemini-ipk/ control conffiles
	@tar --owner=0 --group=0 -czvf /tmp/.pokemini-ipk/data.tar.gz -C /tmp/.pokemini-ipk/root/ .
	@echo 2.0 > /tmp/.pokemini-ipk/debian-binary
	@ar r pokemini/pokemini.ipk /tmp/.pokemini-ipk/control.tar.gz /tmp/.pokemini-ipk/data.tar.gz /tmp/.pokemini-ipk/debian-binary

clean:
	-rm -f $(BUILDOBJS) $(TARGET) pokemini/pokemini.ipk
	-rm -rf $(BUILD)

