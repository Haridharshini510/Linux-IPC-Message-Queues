# Linux-IPC-Message-Queues
Linux IPC-Message Queues

# AIM:
To write a C program that receives a message from message queue and display them

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux message queues API 

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

## C program that receives a message from message queue and display them
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/ipc.h>
#include <sys/msg.h>

struct mesg_buffer {
    long mesg_type;
    char mesg_text[100];
} message;

int main() {
    key_t key;
    int msgid;

    // Generate same key as writer
    key = ftok("progfile", 65);
    if (key == -1) {
        perror("ftok");
        exit(1);
    }

    // Access existing message queue
    msgid = msgget(key, 0666 | IPC_CREAT);
    if (msgid == -1) {
        perror("msgget");
        exit(1);
    }

    printf("Message Queue ID: %d\n", msgid);

    // Receive message
    if (msgrcv(msgid, &message, sizeof(message.mesg_text), 1, 0) == -1) {
        perror("msgrcv");
        exit(1);
    }

    printf("Message received: %s\n", message.mesg_text);

    // Remove the queue after reading
    msgctl(msgid, IPC_RMID, NULL);

    return 0;
}

```

## OUTPUT

<img width="711" height="375" alt="Screenshot 2025-11-09 122740" src="https://github.com/user-attachments/assets/e2ac2312-5251-44f9-9b1e-e1e923368db0" />

<img width="520" height="337" alt="Screenshot 2025-11-09 122749" src="https://github.com/user-attachments/assets/235cefe3-791e-4136-b3a8-3711a2651452" />








# RESULT:
The programs are executed successfully.
