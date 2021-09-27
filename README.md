# PIPEX:
This project aims to be a replicant of | in bash.


# Authorized Functions:

## FILES:
### Open(2): int open(const char *pathname, int flags);

### Close(2): int close(int fd);

### Read(2): ssize_t read(int fd, void *buf, size_t count); 

### Write(2): ssize_t write(int fd, const void *buf, size_t count);

### Access(2): int access(const char *pathname, int mode);
Access() checks if the current process can access the file pathname.If pathname is a symbolic link, it is dereferenced.
The access() function receives a mode:
 - F_OK: Checks if file exists.
 - R_OK: Checks if file exists and has read permision.
 - W_OK: Checks if file exists and has write permision.
 - X_OK: Checks if file exists and has execution permision.

### Unlink(2): int unlink(const char *pathname);
Unlink() deletes a name from the filesystem.
 - If this name is the last link to a file and no processes have the file open, the file is deleted and
the space visible and ready to reuse.
 - If the name is the last link... but there is any process within the file is open, the file will exist
until the last FD refering to that file closes.
 - If the name referred to a symbolic link, the link is removed.
 - If the name is refered to a socket, FIFO or device, the name for it is removed but proccess still has
access to this object.

## PROCCESS:

### Pipe(2): int pipe(int pipefd[2]);
Pipe() creates a pipe, a unidirectional data channel that can be used for interprocess communication.
The array pipefd is used to return two file descriptors referring to the ends of the pipe. pipefd[0] refers to the read end of the pipe.
pipefd[1] refers to the write end of the pipe. Data written to the write end of the pipe is buffered by the kernel until it is read from the read end of the pipe.
On success, zero is returned. On error, -1 is returned, errno is set to indicate the error, and pipefd is left unchanged.

### Dup(2): int dup(int oldfd);
Dup() system call allocates a new file descriptor that refers to the same open file description as the oldfd.
The new file descriptor number is guaranteed to be the lowest-numbered file descriptor that was unused in the calling process.
After a successful return, the old and new file descriptors may be used interchangeably.

### Dup2(2): int dup2(int oldfd, int newfd);
Dup2() performs the same call as Dup(). the difference is that instead of take automaticly the lowest file descriptor unused by the system,
use as the new FD the newfd parameter. 
If the file descriptor newfd was previously open, it is closed before being reused; the close is performed silently (i.e., any errors during the close are not reported by dup2()). The steps of closing and reusing the file descriptor newfd are performed atomically.  This is important, because trying to implement equivalent functionality using close(2) and dup() would be subject to race conditions.

### Wait(2): pid_t wait(int *wstatus);
Wait() system call is use to wait for states changes in a child process, and to obtain info about.A state change is considered to be:
 - the child terminated;
 - the child was stopped by a signal;
 - the child was resumed by a signal 
#### Returns: 
 - < -1 :  meaning wait for any child process whose process group ID is equal to the absolute value of pid.
 - -1 : meaning wait for any child process.
 - 0 : meaning wait for any child process whose process group ID is equal to that of the calling process at the time of the call to waitpid().
 - > 0 : meaning wait for the child process ID is equal to the value of pid.

### Execve(2): int execve(const char *pathname, char *const argv[], char *const envp[]);
Execve() executes the program pointed to by pathname. pathname must be a binary executable file or a script starting with the #! interpreter line.
The argv array contains the argument list. The first argument in argv is the name of the program, and the remaining arguments are the command-line arguments and ended with a NULL.
### Fork(2): pid_t fork(void);
Fork() system call creates a new process by duplicating the calling process. the new process is referred to as the child process, and the calling process is referred to as the parent process. 

## MEMORY:
### Malloc(3): void *malloc(size_t size);
### free(3): void free(void *ptr);

