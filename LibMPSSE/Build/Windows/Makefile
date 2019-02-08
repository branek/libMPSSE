# Makefile to build the libMPSSE.dll
# --- macros

ifeq ($(OS),Windows_NT)
	FILE_NAME = libMPSSE.dll
	DEL_CMD = del
	F_PIC =
	WL_PARAM = -Wl,--output-def,libMPSSE.def,--out-implib,libMPSSE.lib
	EXTERNAL_INC_DIR = ../../../External/Windows/CDMv2.12.28
	CCFOR = WIN
else
	FILE_NAME = libMPSSE.so
	DEL_CMD = rm
	F_PIC = -fPIC
	WL_PARAM =
    UNAME_P := $(shell uname -p)
    ifneq ($(filter arm%,$(UNAME_P)),)
		EXTERNAL_INC_DIR = ../../../External/Linux/libftd2xx1.4.8-armhf/release
		CCFOR = ARM
	else
		EXTERNAL_INC_DIR = ../../../External/Linux/libftd2xx1.4.8/release
		CCFOR = LINUX
    endif
endif

COMMON_INC_DIR = ../../Common/inc
INFRA_INC_DIR = ../../Infra/inc
MIDDLE_INC_DIR = ../../MiddleLayer/inc
I2C_INC_DIR = ../../TopLayer/I2C/inc
SPI_INC_DIR = ../../TopLayer/SPI/inc

ALL_INC_DIR = -I$(EXTERNAL_INC_DIR) -I$(INFRA_INC_DIR) -I$(COMMON_INC_DIR) -I$(MIDDLE_INC_DIR) -I$(SPI_INC_DIR) -I$(I2C_INC_DIR)

COMMON_SRC_DIR = ../../Common/src
INFRA_SRC_DIR = ../../Infra/src
MIDDLE_SRC_DIR = ../../MiddleLayer/src
I2C_SRC_DIR = ../../TopLayer/I2C/src
SPI_SRC_DIR = ../../TopLayer/SPI/src

ALL_SRC_DIR = -I$(INFRA_SRC_DIR) -I$(COMMON_SRC_DIR) -I$(MIDDLE_SRC_DIR) -I$(SPI_SRC_DIR) -I$(I2C_SRC_DIR)

#Put macros here
#MACROS = -DINFRA_DEBUG_ENABLE
MACROS = 

CC=gcc
AR=ar
LD=ld
#use only o0 during debugging

#flags for release mode compilation
CFLAGS= -O3 -Wall $(MACROS) $(ALL_INC_DIR)

#without profiling & coverage
#CFLAGS=  -g -O0 -Wall $(MACROS) $(ALL_INC_DIR)

#with profiling & coverage
#CFLAGS=  -g -O0 -Wall -fprofile-arcs -ftest-coverage D$(MACROS) $(ALL_INC_DIR)

OBJECTS= ftdi_infra.o ftdi_mid.o ftdi_spi.o ftdi_i2c.o

LIBS = -L /MinGW/lib -ldl

# --- targets
all:    libMPSSE
libMPSSE:   $(OBJECTS)
		$(CC)  -o $(FILE_NAME) -shared $(OBJECTS) $(LIBS) $(WL_PARAM)
		$(AR) -rcs libMPSSE.a $(OBJECTS)

ftdi_infra.o: $(INFRA_INC_DIR)
		$(CC) $(CFLAGS) -c $(F_PIC) $(INFRA_SRC_DIR)/ftdi_infra.c
		
ftdi_mid.o: $(MIDDLE_INC_DIR)
		$(CC) $(CFLAGS) -c $(F_PIC) $(MIDDLE_SRC_DIR)/ftdi_mid.c
		
ftdi_i2c.o: $(I2C_INC_DIR)
		$(CC) $(CFLAGS) -c $(F_PIC) $(I2C_SRC_DIR)/ftdi_i2c.c

ftdi_spi.o: $(SPI_INC_DIR)
		$(CC) $(CFLAGS) -c $(F_PIC) $(SPI_SRC_DIR)/ftdi_spi.c

# --- remove binary and executable files
#clean:
#		del -f tst $(OBJECTS)

clean :
ifeq ($(OS),Windows_NT)
	del *.i *.o *.exe *.bak *.txt *.dll *.lib *.a *.def *.exp
else
	rm *.i *.o *.exe *.bak *.txt *.dll *.lib *.a *.def *.exp
endif



