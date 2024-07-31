obj-m := helloworld-params.o helloworld.o

# use default path for the kernel build where the current kernel is installed
KERNELDIR ?= /lib/modules/$(uname -r)/build

all default: modules
install: modules_install

modules modules_install help clean:
	$(MAKE) -C $(KERNELDIR) M=$(shell pwd) $@


sign:
	${KERNELDIR}/scripts/sign-file sha512 ../keys/signing_key.pem ../keys/signing_key.x509 helloworld.ko

insert:
	sudo insmod helloworld.ko

remove:
	sudo rmmod helloworld.ko


