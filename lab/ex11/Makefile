all:
	arm-none-linux-gnueabi-as -mcpu=arm926ej-s -g start.S -o start.o 
	arm-none-linux-gnueabi-as -mcpu=arm926ej-s -g low_level_init.S -o low_level_init.o
	arm-none-linux-gnueabi-gcc -c -mcpu=arm926ej-s -g board.c -o board.o  #-nostdlib
	arm-none-linux-gnueabi-gcc -c -mcpu=arm926ej-s -g main.c -o  main.o -static

	arm-none-linux-gnueabi-ld -T map.lds main.o board.o low_level_init.o start.o -o test.elf
	arm-none-linux-gnueabi-objcopy -O binary test.elf test.bin
	
test:	all
	qemu-system-arm -M versatilepb -m 128M -gdb tcp::1024 -serial stdio -kernel test.elf -S &
	sleep 3
	ddd --debugger arm-none-linux-gnueabi-gdb test.elf
run:	all
	qemu-system-arm -M versatilepb -serial stdio -semihosting -kernel test.bin

clean:
	rm -rf *.o test.elf test.bin
