AS=../cross/bin/i686-elf-as
CC=../cross/bin/i686-elf-gcc
SYS=i386

all: boot kernel 
	@make link

install:
	@grub-file --is-x86-multiboot myos.bin
	@cp myos.bin isodir/boot/myos.bin
	@make gen-iso

boot:
	@echo "compiling boot.s"
	@$(AS) boot.s -o boot.o

kernel:
	@echo "compiling kernel.c"
	@$(CC) -c kernel.c -o kernel.o -std=gnu99 -ffreestanding -O2 -Wall -Wextra

link:
	@echo "linking boot and kernel"
	@$(CC) -T linker.ld -o myos.bin -ffreestanding -O2 -nostdlib boot.o kernel.o -lgcc

gen-iso:
	@grub-mkrescue -o myos.iso isodir

run:
	qemu-system-$(SYS) -cdrom myos.iso

test:
	qemu-system-$(SYS) -kernel myos.bin

clean:
	rm  *.o myos.* ./**/*.bin
