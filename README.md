# Linux-IPC--Pipes
Linux-IPC-Pipes


# Ex03-Linux IPC - Pipes

# AIM:
To write a C program that illustrate communication between two process using unnamed and named pipes

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux Process API - pipe(), fifo()

### Step 3:

Testing the C Program for the desired output. 

# PROGRAM:

## C Program that illustrate communication between two process using unnamed pipes using Linux API system calls
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <string.h>

int main() {
    char *fifo = "/tmp/myfifo";
    char write_msg[] = "Hello through FIFO!";
    char read_msg[100];

    // Create FIFO
    if (mkfifo(fifo, 0666) == -1) {
        perror("mkfifo failed");
    }

    pid_t pid = fork();
    if (pid < 0) {
        perror("fork failed");
        return 1;
    }

    if (pid > 0) { // Parent writes
        int fd = open(fifo, O_WRONLY);
        write(fd, write_msg, strlen(write_msg)+1);
        close(fd);
    } else { // Child reads
        int fd = open(fifo, O_RDONLY);
        read(fd, read_msg, sizeof(read_msg));
        printf("Child received: %s\n", read_msg);
        close(fd);
    }

    // Remove FIFO
    unlink(fifo);
    return 0;
}


```




## OUTPUT

<img width="914" height="302" alt="Screenshot 2026-03-15 143616" src="https://github.com/user-attachments/assets/2b985c56-080a-4f8a-939a-064e77120e54" />


## C Program that illustrate communication between two process using named pipes using Linux API system calls
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <string.h>

int main() {
    char *fifo = "/tmp/myfifo";         // FIFO file path
    char write_msg[] = "Hello from Parent via FIFO!";
    char read_msg[100];

    // Step 1: Create the named pipe (FIFO)
    if (mkfifo(fifo, 0666) == -1) {
        perror("mkfifo failed"); // If FIFO already exists, it may fail
    }

    // Step 2: Fork a child process
    pid_t pid = fork();
    if (pid < 0) {
        perror("fork failed");
        return 1;
    }

    if (pid > 0) { 
        // Parent process writes to FIFO
        int fd = open(fifo, O_WRONLY);     // Open FIFO for writing
        write(fd, write_msg, strlen(write_msg)+1);
        close(fd);
        printf("Parent wrote message to FIFO.\n");
    } else {
        // Child process reads from FIFO
        int fd = open(fifo, O_RDONLY);     // Open FIFO for reading
        read(fd, read_msg, sizeof(read_msg));
        printf("Child received: %s\n", read_msg);
        close(fd);
    }

    // Step 3: Remove FIFO file after use
    unlink(fifo);

    return 0;
}



```




## OUTPUT

<img width="991" height="261" alt="Screenshot 2026-03-15 143821" src="https://github.com/user-attachments/assets/511822c3-038f-4231-985c-6cc6275d5e57" />


# RESULT:
The program is executed successfully.
