#Makefile for STM8 Examples with SDCC compiler
#Author:	Saeid Yazdani
#Website:	WWW.EMBEDONIX.COM
#Copyright 	2016
#LICENSE:	GNU-LGPL

.PHONY: all clean

# Compiler
CC = sdcc
OBJCOPY = stm8-objcopy
SIZE = stm8-size

#Platform
PLATFORM = stm8

# Target name
PNAME = main

# External libraries
IDIR = ../external/STM8S_StdPeriph_Lib/inc
SDIR = ../external/STM8S_StdPeriph_Lib/src

# In case you ever want a different name for the main source file
MAINSRC = main.c

ELF_SECTIONS_TO_REMOVE = -R DATA -R INITIALIZED -R SSEG -R .debug_line -R .debug_loc -R .debug_abbrev -R .debug_info -R .debug_pubnames -R .debug_frame

# These are the sources that must be compiled to .rel files:
EXTRASRCS = \
    $(SDIR)/stm8s_iwdg.c \
	$(SDIR)/stm8s_itc.c \
	$(SDIR)/stm8s_clk.c \
	$(SDIR)/stm8s_gpio.c \
	$(SDIR)/stm8s_uart2.c \
	$(SDIR)/stm8s_tim1.c \
    $(SDIR)/stm8s_tim2.c \
	$(SDIR)/stm8s_tim3.c \
	$(SDIR)/stm8s_exti.c \
	$(SDIR)/stm8s_adc1.c \
	$(SDIR)/stm8s_flash.c \
	system.c \
	motor.c \
	app.c \
	adc.c \
	brake.c \
	throttle.c \
	pas.c \
	torque_sensor.c \
	watchdog.c \
	utils.c
	
HEADERS = \
	interrupts.h \
	pins.h \
	system.h \
	adc.h \
	brake.h \
	motor.h \
	pas.h \
	throttle.h \
	torque_sensor.h \
	watchdog.h \
	app.h \
	utils.h

# The list of .rel files can be derived from the list of their source files
RELS = $(EXTRASRCS:.c=.rel)

INCLUDES = -I$(IDIR) -I. -I../
CFLAGS   = -m$(PLATFORM) -Ddouble=float --std-c99 --nolospre 
ELF_FLAGS = --out-fmt-ihx
LIBS     = 

# This just provides the conventional target name "all"; it is optional
# Note: I assume you set PNAME via some means not exhibited in your original file
all: $(PNAME) hex

# How to build the overall program
$(PNAME): $(MAINSRC) $(RELS)
	$(CC) $(INCLUDES) $(CFLAGS) $(ELF_FLAGS) $(LIBS) $(MAINSRC) $(RELS)
# $(SIZE) $(PNAME).elf
# $(OBJCOPY) -O binary $(ELF_SECTIONS_TO_REMOVE) $(PNAME).elf $(PNAME).bin
# $(OBJCOPY) -O ihex $(ELF_SECTIONS_TO_REMOVE) $(PNAME).elf $(PNAME).hex

# How to build any .rel file from its corresponding .c file
# GNU would have you use a pattern rule for this, but that's GNU-specific
%.rel: %.c $(HEADERS)
	$(CC) -c $(INCLUDES) $(CFLAGS) $(ELF_FLAGS) $(LIBS) -o$< $<

# Suffixes appearing in suffix rules we care about.
# Necessary because .rel is not one of the standard suffixes.
.SUFFIXES: .c .rel

# flash:
#	stm8flash -cstlinkv2 -pstm8s105?6 -w$(PNAME).ihx

hex:
	@cmd /C tohex.bat

clean:
	@cmd /C clean.bat
	@echo "Clean Finished"
