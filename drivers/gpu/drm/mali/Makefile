#
# This confidential and proprietary software may be used only as
# authorised by a licensing agreement from ARM Limited
# (C) COPYRIGHT 2010, 2012 ARM Limited
# ALL RIGHTS RESERVED
# The entire notice above must be reproduced on all authorised
# copies and copies may only be made to the extent permitted
# by a licensing agreement from ARM Limited.
#

MALI_DRM_FILE_PREFIX =

# For each arch check: CROSS_COMPILE , KDIR , CFLAGS += -DARCH

ARCH ?= arm
## @note Should allow overriding of building MALI DRM for non-debug:
EXTRA_CFLAGS += -DDEBUG

DRM_SYMVERS_FILE = ../drm/Module.symvers
KBUILD_EXTRA_SYMBOLS = $(KBUILD_EXTMOD)/$(DRM_SYMVERS_FILE)


# linux build system integration

ifneq ($(KERNELRELEASE),)
# Inside the kernel build system

EXTRA_CFLAGS += -I$(KBUILD_EXTMOD) -I$(KBUILD_EXTMOD)/include -I$(KBUILD_EXTMOD)/../drm/include/

SRC +=	$(MALI_DRM_FILE_PREFIX)mali/mali_drv.c \
	$(MALI_DRM_FILE_PREFIX)mali/mali_mm.c

# Selecting files to compile by parsing the config file

#MODULE:=mali_drm.ko

obj-y := $(SRC:.c=.o)
#$(MODULE:.ko=.o)
#$(MODULE:.ko=-y) := $(SRC:.c=.o)
else
# Outside the kernel build system

# Get any user defined KDIR-<names> or maybe even a hardcoded KDIR
-include KDIR_CONFIGURATION
# Define host system directory
KDIR-$(shell uname -m):=/lib/modules/$(shell uname -r)/build

ifeq ($(ARCH), arm)
	# when compiling for ARM we're cross compiling
	export CROSS_COMPILE ?= arm-none-linux-gnueabi-
	# default to Virtex5
	CONFIG ?= pb-virtex5
else
	# Compiling for the host
	CONFIG ?= $(shell uname -m)
endif

# default cpu to select
CPU ?= ct11mp

# look up KDIR based om CPU selection
KDIR ?= $(KDIR-$(CPU))
ifeq ($(KDIR),)
$(error No KDIR found for platform $(CPU))
endif

all:
	$(MAKE) ARCH=$(ARCH) -C $(KDIR) M=$(CURDIR) modules

kernelrelease:
	$(MAKE) -C $(KDIR) kernelrelease

clean:
	$(MAKE) ARCH=$(ARCH) -C $(KDIR) M=$(CURDIR) clean

endif
