# Operating-System-Lab3-System-Calls(ubuntu OS)
### Introduction 
A system call is just what its name implies—a request for the operating system to do 
something on behalf of the user’s program 
- open() 
- read() 
- write() 
- close() 
- wait() 
- fork() 
- exec() 
### Objective 
- To be familiar with system calls. 
- To be able to use system calls. 
- To be able to read/write. 
- To be able to open/close. 
- To be able to fork and execute. 
### Concept Map 
A system call is just what its name implies—a request for the operating system to do 
something on behalf of the user’s program 
### open() 
open() lets you open a file for reading, writing, or reading and writing. Example of this system 
call is as following.
int open(file_name, mode) 
Where file_name is a pointer to the character string that names the file and mode defines the 
file's access permissions if the file is being created. 
### Read() and Write() 
The read() system call does all input and the write() system call does all output. When used 
together, they provide all the tools necessary to do input and output. Both read() and write() 
take three arguments. 
#### Their prototypes are:
 `int read(file_descriptor, buffer_pointer, 
transfer_size) int file_descriptor; char *buffer_pointer; 
unsigned transfer_size; 
int write(file_descriptor, buffer_pointer,
transfer_size) 
int file_descriptor; 
char *buffer_pointer; 
unsigned transfer_size;`.
Where file_descriptor identifies the I/O channel, buffer_pointer points to the area in memory 
where the data is stored for a read() or where the data is taken for a write(), and transfer_size 
defines the maximum data which is going to be written. 
### close() 
To close a channel, use the close() system call. The prototype for the close() system call is: 

`int close(file_descriptor)` 

`int file_descriptor;` 


## Requirments

open the terminal in linux/ubuntu by pressing `ctrl+alt+t` to open a terminal.Check the version of `gcc and g++ ` by

```bash
  gcc --version
  g++ --version
```

If `g++ or gcc` is not install in your system then use thse commands to install in your system.

```bash
  sudo apt install gcc
  sudo apt install g++
```

When the installation will complete.you can confirm it by
 
`gcc --version`  
`g++ --version`

One additional command to `update and upgrade your system`

```bash
  sudo apt install update && sudo apt install upgrade
                   or
  sudo apt-get install update && sudo apt-get install upgrade
```

Now you are ready to go.

Create folder in a directory in which you want to work  
Go to directory by typing `cd directory-name`.Like `cd wajahat`.Now create a folder in that directory by 
```
mkdir foldername 
```
Like in mine case
```
mkdir Lab3
```

### Implementation of open(), read(), write() and close() functions

```c++
#include<stdio.h> 
#include<unistd.h> 
#include<fcntl.h> 
int main() 
{ 
int fd; 
char buffer[80]; 
static char message[] = "Hello world"; 
fd = open("myfile.txt",O_RDWR); 
if (fd != -1) 
{ 
printf("myfile opened for read/write access\n"); 
write(fd, message, sizeof(message)); 
lseek(fd, 5, 0); 
/* go back to the beginning of the file */ 
read(fd, buffer, sizeof(message)); 
printf(" %s was written to myfile \n", buffer); 
close (fd); 
} 
else 
{ 
printf(" Error in openng myfile \n"); 
} 
}

```


## Next step
- open the terminal , go to that directory and create a file by command
- First screenshot is go to directory.`ls view the current directory all folder and file therefore i use the ls`
```bash
  cat > fileName.cpp
```
- Then paste the code in the terminal.After pasting the code in termianl press `ctrl+c` to leave the file.You can view the file by command `cat fileName` file name must me with extension .Like in mine case `cat oRWC.cpp`.
- Now create in empty file with name `touch myfile.txt`.Touch and cat both commands use to create a file.
### Now we are ready to run this file
#### commands
```bash
  g++ fileName.cpp -o fileNameWithoutExtension
```
#### Like in mine 
```bash
  g++ oRWC.cpp -o oRWC
```
By this command your file is compile. Now you can view the output of command 
```bash
  ./fileNameWithoutExtension
```
#### Like in mine 
```bash
 ./oRWC
```
## fork() 
When the fork system call is executed, a new process is created which consists of a copy of 
the address space of the parent. 
The return code for fork is zero for the child process and the process identifier of child is 
returned to the parent process. On success, both processes continue execution at the instruction 
after the fork call. On failure, -1 is returned to the parent process. 
Implementing fork system call using C program
```bash
#include<stdio.h> 
#include<unistd.h> 
#include<fcntl.h> 
void main() 
{ 
pid_t pid; 
pid = fork(); 
if (pid == 0) 
printf("\n I'm the child process"); 
else if (pid > 0) 
printf("\n I'm the parent process. My child pid is %d", pid); 
else 
perror("error in fork"); 
} 
```
### Steps for this task
- create a file as we discussed above and this program run in the same way as above program
## wait() 

The wait system call suspends the calling process until one of its immediate children 
terminates. If the call is successful, the process ID of the terminating child is returned. 
Zombie process—a process that has terminated but whose exit status has not yet been 
received by its parent process. The process will remain in the operating system’s process table 
as a zombie process, indicating that it is not to be scheduled for further execution. 
But that it cannot be completely removed (and its process ID cannot be reused) 
`pid_t wait(int *status);` 
Where status is an integer value where the UNIX system stores the value returned by child 
process. 
### Implementing wait system call using C program
```bash
#include <stdio.h> 
void main() 
{ 
int pid, status; 
pid = fork(); 
if(pid == -1) 
{ 
printf(“fork failed\n”); 
exit(1); 
} 
if(pid == 0) 
{ 
/* Child */ 
printf(“Child here!\n”); 
} 
else 
{ 
/* Parent */ 
wait(&status); 
printf(“Well done kid!\n”); 
} 
}
``` 
## exec() 
Typically, the exec system call is used after a fork system call by one of the two processes to 
replace the process’ memory space with a new executable program. The new process image 
is constructed from an ordinary, executable file. There can be no return from a successful exec 
because the calling process image is overlaid by the new process image. 
Takes the path name of an executable program (binary file) as its first argument. The rest of 
the arguments are a list of command line arguments to the new program (argv[]). The list is 
terminated with a null pointer: `execl("a.out", "a.out”,NULL)`
## Practice Tasks 
### Practice Task 1 
Write your university card’s details in a text file using write system call. Now read the contents 
of the file using read system call. 
```bash
#include<stdio.h> 
#include<unistd.h> 
#include<fcntl.h> 
int main() 
{ 
int fd; 
char buffer[80]; 
static char message[] = "Hello Dear your details are given below"; 
fd = open("mydetails.txt",O_RDWR); 
if (fd != -1) 
{ 
printf("myfile opened for read/write access \n"); 
write(fd, message, sizeof(message)); 
lseek(fd, 5, 0); 
/* go back to the beginning of the file */ 
read(fd, buffer, sizeof(message)); 
printf(" %s was written to myfile \n", buffer); 
close (fd); 
} 
else 
{ 
printf(" Error in openng myfile \n"); 
} 
}
```
Create a file with name mydetails.txt in same directory.Use the following command.

```bash
cat > mydetails.txt
``` 
After entring details in your terminal then press `ctrl+c` .After this run the following command

```
g++ fileNameWithExtension -o fileNameWithoutExtension
```
After compiling the file run the following command
```
./fileNameWithoutExtension
```
After this to view your details use the following command
```
cat mydetails.txt
     or
cat fileName.txt
```
This command show details you enter in your file.
### Practice Task 2 
Write a code to start a process and create its child processes. Now display details of these 
processes one by one using system calls.
```
#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>

int main() {
   pid_t pid, child, children;
   pid = getpid();
   pid = fork();

   if (pid < 0) {
      perror("fork() failure\n");
      return 1;
   }

   // Child process
   if (pid == 0) {
      printf("This is child process\n");
      child = getpid();
      children = getppid();
      printf("Child id is %d and children is %d\n", child, children);
   } else { // Parent process 
      sleep(2);
      printf("This is parent process\n");
}
}
```
To run this use the following commands.

```
g++ fileNameWithExtension -o fileNameWithoutExtension
```
To run the compile file use the following commands
```
./fileNameWithoutExtension
```



## Next step
- open the terminal , go to that directory and create a file by command
- First screenshot is go to directory.`ls view the current directory all folder and file therefore i use the ls`
```bash
  cat > fileName.cpp
```
- Then paste the code in the terminal.After pasting the code in termianl press `ctrl+c` to leave the file.You can view the file by command `cat fileName` file name must me with extension .Like in mine case `cat oRWC.cpp`.
- Now create in empty file with name `touch myfile.txt`.Touch and cat both commands use to create a file.
### Now we are ready to run this file
#### commands
```bash
  g++ fileName.cpp -o fileNameWithoutExtension
```
#### Like in mine 
```bash
  g++ oRWC.cpp -o oRWC
```
By this command your file is compile. Now you can view the output of command 
```bash
  ./fileNameWithoutExtension
```
#### Like in mine 
```bash
 ./oRWC
```
## fork() 
When the fork system call is executed, a new process is created which consists of a copy of 
the address space of the parent. 
The return code for fork is zero for the child process and the process identifier of child is 
returned to the parent process. On success, both processes continue execution at the instruction 
after the fork call. On failure, -1 is returned to the parent process. 
Implementing fork system call using C program
```bash
#include<stdio.h> 
#include<unistd.h> 
#include<fcntl.h> 
void main() 
{ 
pid_t pid; 
pid = fork(); 
if (pid == 0) 
printf("\n I'm the child process"); 
else if (pid > 0) 
printf("\n I'm the parent process. My child pid is %d", pid); 
else 
perror("error in fork"); 
} 
```
### Steps for this task
- create a file as we discussed above and this program run in the same way as above program
## wait() 

The wait system call suspends the calling process until one of its immediate children 
terminates. If the call is successful, the process ID of the terminating child is returned. 
Zombie process—a process that has terminated but whose exit status has not yet been 
received by its parent process. The process will remain in the operating system’s process table 
as a zombie process, indicating that it is not to be scheduled for further execution. 
But that it cannot be completely removed (and its process ID cannot be reused) 
`pid_t wait(int *status);` 
Where status is an integer value where the UNIX system stores the value returned by child 
process. 
### Implementing wait system call using C program
```bash
#include <stdio.h> 
void main() 
{ 
int pid, status; 
pid = fork(); 
if(pid == -1) 
{ 
printf(“fork failed\n”); 
exit(1); 
} 
if(pid == 0) 
{ 
/* Child */ 
printf(“Child here!\n”); 
} 
else 
{ 
/* Parent */ 
wait(&status); 
printf(“Well done kid!\n”); 
} 
}
``` 
## exec() 
Typically, the exec system call is used after a fork system call by one of the two processes to 
replace the process’ memory space with a new executable program. The new process image 
is constructed from an ordinary, executable file. There can be no return from a successful exec 
because the calling process image is overlaid by the new process image. 
Takes the path name of an executable program (binary file) as its first argument. The rest of 
the arguments are a list of command line arguments to the new program (argv[]). The list is 
terminated with a null pointer: `execl("a.out", "a.out”,NULL)`
## Practice Tasks 
### Practice Task 1 
Write your university card’s details in a text file using write system call. Now read the contents 
of the file using read system call. 
```bash
#include<stdio.h> 
#include<unistd.h> 
#include<fcntl.h> 
int main() 
{ 
int fd; 
char buffer[80]; 
static char message[] = "Hello Dear your details are given below"; 
fd = open("mydetails.txt",O_RDWR); 
if (fd != -1) 
{ 
printf("myfile opened for read/write access \n"); 
write(fd, message, sizeof(message)); 
lseek(fd, 5, 0); 
/* go back to the beginning of the file */ 
read(fd, buffer, sizeof(message)); 
printf(" %s was written to myfile \n", buffer); 
close (fd); 
} 
else 
{ 
printf(" Error in openng myfile \n"); 
} 
}
```
Create a file with name mydetails.txt in same directory.Use the following command.

```bash
cat > mydetails.txt
``` 
After entring details in your terminal then press `ctrl+c` .After this run the following command

```
g++ fileNameWithExtension -o fileNameWithoutExtension
```
After compiling the file run the following command
```
./fileNameWithoutExtension
```
After this to view your details use the following command
```
cat mydetails.txt
     or
cat fileName.txt
```
This command show details you enter in your file.
### Practice Task 2 
Write a code to start a process and create its child processes. Now display details of these 
processes one by one using system calls.
```
#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>

int main() {
   pid_t pid, child, children;
   pid = getpid();
   pid = fork();

   if (pid < 0) {
      perror("fork() failure\n");
      return 1;
   }

   // Child process
   if (pid == 0) {
      printf("This is child process\n");
      child = getpid();
      children = getppid();
      printf("Child id is %d and children is %d\n", child, children);
   } else { // Parent process 
      sleep(2);
      printf("This is parent process\n");
}
}
```
To run this use the following commands.

```
g++ fileNameWithExtension -o fileNameWithoutExtension
```
To run the compile file use the following commands
```
./fileNameWithoutExtension
```



## Authors

- [@Twitter](https://twitter.com/WajahatAli0981)
- [@Linkedin](https://www.linkedin.com/in/wajahat-ali-basharat/)
- [@Github](https://github.com/WajahatAliBasharat073)


Follow me for more interesting blogs
   
