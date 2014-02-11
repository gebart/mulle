################################################################################
#
# Makefile for the Freescale Kinetis K60 / ARM Cortex-M4
#
################################################################################

# Name of project/output file:

TARGET = leds

# List your asm files here (minus the .s):

ASM_PIECES = startcode

# List your c files here (minus the .c):

C_PIECES = leds

# Define Hardware Platform

TOOLCHAIN_PREFIX = arm-none-eabi-
CC = $(TOOLCHAIN_PREFIX)gcc
#CC = clang -target armv7m-none-eabi
AS = $(TOOLCHAIN_PREFIX)as
LD = $(TOOLCHAIN_PREFIX)ld
GDB = gdb
OBJDUMP = $(TOOLCHAIN_PREFIX)objdump
SZ = $(TOOLCHAIN_PREFIX)size

ASM_FLAGS = -g
ASM_FILES = ${ASM_PIECES:%=%.s}
ASM_O_FILES = ${ASM_FILES:%.s=%.o}

OPT_LEVEL = 0

INCLUDES = -I../../include
C_FLAGS  = -Wall -c -ggdb -MD -O${OPT_LEVEL} $(DEFINES) $(INCLUDES) -ffunction-sections -fdata-sections
C_FILES = ${C_PIECES:%=%.c}
C_O_FILES = ${C_FILES:%.c=%.o}

O_FILES = ${ASM_O_FILES} ${C_O_FILES}

CPU_FLAGS = -mcpu=cortex-m4 -mthumb

LD_SCRIPT = ../../ldscripts/mulle.ld

# nostartfiles prevents the toolchain from including startup routines.
LD_FLAGS = -nostartfiles -Wl,-Map=${TARGET}.map,--gc-sections

all: ${TARGET}.axf
	@${OBJDUMP} -DS ${TARGET}.axf >| ${TARGET}.out.s
	@${CC} --version

${TARGET}.axf: ${O_FILES}
	@echo
	${CC} ${O_FILES} ${LIBS} -T ${LD_SCRIPT} ${LD_FLAGS} -o ${TARGET}.axf
	${SZ} ${TARGET}.axf

%.o: %.s
	${AS} ${ASM_FLAGS} ${CPU_FLAGS} -o $@ $<

%.o: %.c
	${CC} ${C_FLAGS} ${CPU_FLAGS} -o $@ $<

clean:
	@echo
	@echo Cleaning up...
	@echo
	-rm -f *.o
	-rm -f *.d
	-rm -f ${TARGET}.axf
	-rm -f ${TARGET}.out.s
	-rm -f ${TARGET}.map
