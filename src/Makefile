#******************************************************************************
# Copyright (C) 2017 by Alex Fosdick - University of Colorado
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are 
# permitted to modify this and use it to learn about the field of embedded
# software. Alex Fosdick and the University of Colorado are not liable for any
# misuse of this material. 
#
#*****************************************************************************

#------------------------------------------------------------------------------
# <Put a Description Here>
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      <Put a description of the supported targets here>
#
# Platform Overrides:
#      <Put a description of the supported Overrides here
#
#------------------------------------------------------------------------------
include sources.mk

TARGET = c1m2
PLATFORM = HOST
OBJS = $(SOURCES:.c=.o)
GFLAGS = -Wall -Werror -g -O0 -std=c99
CPPFLAGS = -E
DEPS = $(SOURCES:.c=.d)
# Platform Overrides
ifeq ($(PLATFORM), MSP432)
	CC = arm-none-eabi-gcc
	LD = arm-none-eabi-ld
	LINKER_FILE = ../msp432p401r.lds
	LDFLAGS =  -Wl,-Map=$(TARGET).map -T $(LINKER_FILE)

	CPU = cortex-m4
	ARCH = thumb
	SPECS = nosys.specs
	FPU = fpv4-sp-d16
	CFLAGS = $(GFLAGS) -mcpu=$(CPU) -m$(ARCH) --specs=$(SPECS) -march=armv7e-m \
            -mfloat-abi=hard -mfpu=$(FPU) -D$(PLATFORM)
	OBJDUMP = arm-none-eabi-objdump

	SIZE = arm-none-eabi-size
else
	CC = gcc
	LD = ld
	LDFLAGS =  -Wl,-Map=$(TARGET).map
	CFLAGS = $(GFLAGS) -D$(PLATFORM)
	SIZE = size
	OBJDUMP = objdump
endif

%.o : %.c
	$(CC) $(INCLUDES) -c $< $(CFLAGS) -o $@
%.i: %.c
	$(CC) $(INCLUDES) $(CPPFLAGS) $< $(CFLAGS) -o $@
%.asm: %.c
	$(CC) $(INCLUDES) -S $< $(CFLAGS) -o $@
%.d: %.c
	$(CC) $(INCLUDES) -M $< $(CFLAGS) -o $@

.PHONY: all
build: all
.PHONY: all
all: $(TARGET)

$(TARGET): $(OBJS) 
	$(CC) $(OBJS) $(CFLAGS) $(LDFLAGS) -o $@.out

.PHONY: compile-all
compile-all: $(OBJS)
	$(CC) $(INCLUDES) $(OBJS) -c $(CFLAGS) -o $(TARGET).o

.PHONY: build
build: $(OBJS) $(DEPS)
	$(CC) $(INCLUDES) $(OBJS) $(CFLAGS) $(LDFLAGS) -o $(TARGET).out
.PHONY: clean
clean:
	rm -f *.o *.i *.map *.out *.asm *.d

