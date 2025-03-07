commad for build the img

mbr:
nasm -o mbr ./src/boot/mbr.S
dd if=mbr of=test.img bs=512 count=1 seek=0 conv=notrunc

loader:
nasm -o loader ./src/boot/loader.S
dd if=loader of=test.img bs=512 count=8 seek=1 conv=notrunc

kernel:
gcc -m32 -nostdlib -nostdinc -fno-builtin -fno-stack-protector -no-pie -fno-pic -c src/main.c -o main.o
ld -m elf_i386 -Tlink.ld -o kernel main.o
dd if=kernel of=test.img bs=512 count=2048 seek=9 conv=notrunc