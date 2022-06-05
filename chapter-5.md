## Chapter 5: File IO: further details

Relationship Between File Descriptors and Open Files

We need to examine three data structures maintained by the kernel:

- the per-process file descriptor table
- the system-wide table of open file descriptions
- the file system i-node table

For each process, the kernel maintains a table of open file descriptors. Each entry in this table records information about a single file descriptor, including:

- a set of flags controlling the operation of the file descriptor (there is just one such flag, the close-on-exec flag)
- a reference to the open file description

The kernel maintains a system-wide table of the file descriptor. (This table is sometimes referred to as the open file table, and its entries are sometimes called open file handles.) An open file description stores all information relating to an open file, including:

- the current file offset 
- status flags specified when opening the file
- the file access mode
- settings related to signal-driven IO
- a reference the the inode object for this file

Each file sytem has a table of inodes for all files residing in the file system. For now, we note that the inode for each file includes the following information:

- file type (e.g., regular file, socker, or FIFO) and permissions
- a pointer to a list of locks held on this file
- various properties of the file, including its size and timestamps relating to different types of file operations.

Duplicating File Descriptors

Using the IO redirection syntax `2>&1` informs the shell that we wish to have standard error (file descriptor 2) redirected to the same place to which standard out (file descriptor 1) is being sent. Thus the following command would send both standard output and standard error to the file `results.log`:

```
./myscript > results.log 2>&1
```

The dev/fd Directory

For each process, the kernel provides the special virtual directory `/dev/fd`. This directory contains filenames of the form `/dev/fd/n`, where n is a number corresponding to one of the open file descriptors for the process. Thus, for example, `/dev/fd/0` is standard input for the process. Opening one of the files in the `/dev/fd` directory is equivalent to duplicating the corresponding file descriptor. Thus, the following statements are equivalent:

```
fd = open("dev/fd/1", O_WRONLY);
fd = dup(1)
```

Their most common use is in the shell. Many user-level commands take filename arguments, and sometimes we would like to put them in a pipeline and have on of the arguments be standard input or output instead. For this purpose, some programs (e.g., diff, ed, tar, and comm) have evolved the hack of using an argument consisting of a single hyphen (-) to mean "use standard input or output (as approriate)" for this filename argument. Thus, to compare a file list from ls against a previously build file list, we might write the following:

```
ls | diff - oldfilelist
```

Using /dev/fd elimenates some difficulties, allowing the specification of standard input, output, and error as filename arguments to any program requiring them. Thus, we can write the previous shell commands as follows:

```
ls | diff /dev/fd/0 oldfilelist
```

As a convenience, the names /dev/stdin, /dev/stdout, and /dev/stderr are provided as symbolic links to, respectively, /dev/fd/0, /dev/fd/1 and /dev/fd/2.

