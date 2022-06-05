## Chapter 2: Fundamental Concepts

Within the file system, each file is marked with _type_, indicating what kind of file it is. One of these file types denotes ordinary data files, which are usually called regular or plain files to distinguish them from other file types. These other file types include pipes, sockets, directories, and symbolic links. 

Symbolic links

Like a normal link, a symbolic link provides an alternative name for a file. But whereas a normal link is a filename-plus-pointer entry in a directory list, a symbolic link is a specially marked file containing the name of another file. 

Programs normally exist in two forms. The first form is the source code, human-readable text consisting of a series of statements written in a programming language such as C. 

A filter is the name often applied to a program that reads its input from stdin, performs some transformation of that input, and writes the transformed data to stdout. Examples of filters: `cat`, `grep`, `tr`, `sort`, `wc`, `sed` and `awk`.

```cpp
int main(int argc, char* argv[])
```

The `argc` variable contains the total number of command-line arguments, and the individual arguments are available as strings pointed to by members of the array `argv`. The first of these strings, `argv[0]`, identifies the name of the program itself. 

A process is logically divided into the following parts, known as _segments_:

- Text: the instructions of the program
- Data: the static variables used by the program
- Heap: an area from which programs can dynamically allocate extra memory
- Stack: a pice of memory that grows and shrinks as functions are called and return and that is used to allocate storage for local variables and function call linkage information.

A process can create a new process using the `fork()` system call. The child process goes on either to execute a different set of functions in the same code as the parent, or, frequently, to use the `execve()` system call to load and execute an entirely new program. 

A deamon is a special-purpose process that is created and handled by the system in the same way as other processes, but which is distinguished by the following characteristics:

- it is long-lived. A daemon process is often started at system boot and remains in existence until the system is shut down.
- It runs in the background, and has no controlling terminal from which it can read input or to which it can write output. 

Examples of daemon processes include `syslogd`, which records messages in the system log, and `httpd`, which serves web pages via the Hypertext Tranfer Protocol (http).

Each process has an environment list, which is a set of environment variables that are maintained withing the user-space memory of the process. Each element of this list consists of a name and an associated value. When a new process is created via `fork()`, it inherits a copy of its parent's environment. Thus the environment provides a mechanism for a parent process to communicate information to a child process. When a process replaces the program that is running using `exec()`, the new program either inherits the environment used by the old program or receives a new environment specified as part of the `exec()` call.

Environment variables are created with the `export` command in most shells, as in the following example:

```
export MYVAR='Hello world'
```

Environment variables are used for a variety of purposes. For example, the shell defines and uses a range of variables that can be accessed by scripts and programs executed from the shell. These include the variable HOME, which specifies the pathname of the user's login directory, and the variable PATH, which specifies a list of directories that the shell should search when looking for programs corresponding to commands entered by the user. 

Memory Mappings

The `mmap()` system call creates a new memory mapping in the calling process' virtual address space. Mappings fall into two categories:

- A file mapping maps a region of a file into the calling process's virtual memory. Once mapped, the file's contents can be accessed by operations on the bytes in the corresponding memory region. The pages of the mapping are automatically loaded from the file as required.
- By contrasts, an anonymous mapping doesn't have a corresponding file. Instead, the pages of the mapping are initialized to 0.

When two or more processes share the same pages, each process may see the changes made by other processes to the contents of the pages, depending on whether the mapping is created as private or shared. When a mapping is private, modifications to the contents of the pages are not visible to other processes and are not carried through to the underlying file. 

Memory mappings serve a wide variety of purposes, including initialization of a process's text segment from the corresponding segment of an executable file, allocation of new (zero-filled) memory, file IO (memory mapped IO), and interprocess communication (via shared mapping).

Linux provides a rich set of mechanisms for interprocess communication (IPC), including the following:

- signals, which are used to indicate that an event has occurred.
- pipes and FIFOs, which can be used to transfer data between processes.
- sockets, which can be used to transfer data from one process to another, either on the same host computer or on different hosts connected by a network.
- file locking, which allows a process to lock regions of a file in order to prevent other processes from reading or updating the file contents.
- message queues, which are used to exchange messages (packets of data) between processes
- semaphores, which are used to synchronize the actions of processes
- shared memory, which allows two or more processes to share a piece of memory

Signals

Each program executed by the shell is started in a new process. For example, the shell creates three processes to execute the following pipeline commands (which displays a list of files in the current working directory sorted by file size)

```
ls -l | sort -k5n | less
```

