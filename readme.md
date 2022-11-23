# Simple-Syscall-Implementation

## Explanation
Firstly add the new syscall to the table of already existing syscalls
in ```build/linux-5.xx.xx/arch/x86/entry/syscalls/syscall_64.tbl``` add

    449 kern_2D_memcpy sys_kern_2D_memcpy

Then implement the same syscall in `sys.c` located at `buiild/linux-5.xx.xx/kernal/sys.c`
    
Where we define the following function 

    SYSCALL_DEFINE4(kern_2D_memcpy, float *, MAT1, float *, MAT2, int, ROW, int, COL)

We take the pointers to the two float matrices where `MAT1` is the destination matrix and `MAT2` is the source matrix. 
We create a new matrix of dimensions ROWxCOL in the kernel space to which we then copy the contents of `MAT2` using `copy_from_user` and then we copy it to `MAT1` using `copy_to_user`
If any of the above are not possible we return `-EFAULT` else return `0` incase of success.

<br>

## Building/Compiling the syscall
After adding the syscalls we need to run the following commands to configure our kernel

    make

    make modules_install

    cp arch/x86_64/boot/bzImage /boot/vmlinuz-linux-5.15.4-gb0ccfee715-dirty
    
    cp System-5.15.4.map System-5.15.4-gb0ccfee715-dirty.map

    mkinitcpio -k 5.15.4-gb0ccfee715-dirty -g /boot/initramfs-linux-5.15.4-gb0ccfee715-dirty.img

    grub-mkconfig -o /boot/grub/grub.cfg

    reboot
<br>

## Test the syscall
Make the test files in any directory and then run

    gcc test.c -o test
    ./test
   



