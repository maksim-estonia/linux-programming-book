## Chapter 4: File IO: The Universal IO Model

Changing the File Offset: lseek()

For each open file, the kernel records a file offset, sometimes also called the read-write offset or pointer. This is the location in the file at which the next read() or write() will commence. The file offset is expressed as an ordinal byte position relative to the start of the file. The first byte of the file is at offset 0.

The lseek() system call adjusts the file offset of the open file referred to by the file descriptor fd, according to the values specified in offset and whence.

Operations outside the Universal IO Model: ioctl()

The ioctl() system call is a general-purpose mechanism for performing file and device operations that fall outside the universal IO model described earlier.

```c
#include <sys/ioctl.h>

int ioctl(int fd, int request, ...);
```

The fd argument is an open file descriptor for the device or file upon which the control operation specified by request is to be performed. Device-specific header files define constants that can be passed in the request argument. 