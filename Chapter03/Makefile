obj-m := cmwq.o \
         tasklet.o \
         hr-timer.o \
         std-timer.o \
         waitqueue.o \
         shared-workqueue.o \
         dedicated-workqueue.o \
         user-invoke.o

KERNELDIR ?= /lib/modules/$(shell uname -r)/build

# Path to the signing key and certificate
KEY_DIR = ../keys
SIGNING_KEY = $(KEY_DIR)/signing_key.pem
SIGNING_CERT = $(KEY_DIR)/signing_key.x509

# Find all .ko files in the current directory
KO_FILES := $(wildcard *.ko)

all default: modules
install: modules_install

modules modules_install help clean:
	$(MAKE) -C $(KERNELDIR) M=$(shell pwd) $@


# Target to sign all .ko files
.PHONY: sign-all
sign-all: $(KO_FILES)
	# @ in front of echo suppresses the echoing of the command being run
	@echo "Moew:" $^ #Moew: helloworld.ko helloworld-params.ko
	@for file in $^; do \
		echo "Signing $$file..."; \
		${KERNELDIR}/scripts/sign-file sha512 $(SIGNING_KEY) $(SIGNING_CERT) $$file; \
	done

insert_%:
	# in Makefile. The $* variable represents the stem matched by the % in the pattern rule
	sudo insmod $*.ko

remove_%:
	sudo rmmod $*.ko


info_%:
	modinfo $*.ko