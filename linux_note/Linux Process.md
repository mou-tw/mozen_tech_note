Types of Processes
These are the types of processes in Linux.

Init Process
The init process is the first process that starts when a Linux or Unix-like system is booted. It precedes other processes that can be used on the CLI, and is started by the kernel. It has a process ID of 1. Orphaned processes use it as an adoptive parent when they are not taken over by other processes.

Parent Process
A process that has produced one or more child processes is known as a parent process in Linux. When a process initiates the initiation of another process, the initiating process is referred to as the parent process, and the newly initiated process is referred to as the child process. The child process and parent process have separate process IDs and operate simultaneously (PIDs).
The environment variables and current working directory are two examples of properties that the child process inherits from the parent process. The child process, on the other hand, has its own memory and resources and is capable of running separately from the parent process.
A hierarchical relationship exists between the parent and child processes, with the parent process at the top and the child process at the bottom.

Child Process
A child process has the ability to produce additional offspring, resulting in a tree-like structure of processes.
Using different inter-process communication (IPC) mechanisms like pipes, sockets, and signals, a parent process can communicate with its child processes and control how they are being executed. The parent process has the option of waiting for the child process to finish before collecting the exit status of the child.
The parent process is in charge of initiating the child process, and it also has the ability to end the child process or send it a signal to do so. The parent process can also keep an eye on the child process, and if it crashes or terminates unexpectedly, it can respond appropriately by restarting the child process or logging an error message.
A shell is an example of a parent process; when a command is run, the shell is thought of as the parent process, and the process that executes the command is thought of as the child process.
In Linux, a process that is created or spawned by another process is referred to as a "child process". Using system calls like fork() or spawn, the parent process creates the child process(). The current working directory and environment variables are among the properties that the child process inherits from the parent process, but it also has its own memory space and resources and the ability to run independently of the parent process.
A distinct process ID (PID) is given to each child process, which is treated as a distinct entity from the parent process. The child process has the ability to run any command or program and can also start new child processes, arranging them in a tree-like structure.
Through the use of various inter-process communication (IPC) mechanisms like pipes, sockets, and signals, a child process can communicate with both its parent and other child processes. The child process has the ability to end either on its own or as a result of the parent process.
When a child process ends, the parent process is alerted and has the opportunity to gather the exit status of the child. The parent process can use this information to determine whether the child process terminated normally or crashed.

The execution of a command in the shell is an example of a child process; the shell is the parent process, and the command is the child process. The child process is independent of the shell and has the ability to create additional child processes.

Orphaned Process
In Linux, an orphaned process is a child process whose parent process has ended or left the system without properly closing the child process down or reaping it. The init process (PID 1), which is in charge of maintaining the process table and eliminating orphaned processes, is typically assigned as the new parent process to the child process when the old one exits.
Orphaned processes can happen when a parent process exits without properly cleaning up its child processes, crashes, or is abruptly terminated. Since they are still listed in the process table but not currently running or using any resources, these processes are frequently referred to as "zombie" processes.
Although orphaned processes do not use any system resources and have no impact on system performance, they will take up space in the process table and prevent the creation of new processes if the system reaches the maximum number of processes permitted.
It is best practice for a parent process to properly close down its child processes when they exit, whether by using the wait() or waitpid() system calls or the SIGCHLD signal. This helps prevent orphaned processes.
The init process is in charge of eliminating zombie processes; to do this, it sends the parent process a periodic SIGCHLD signal, instructing it to wait for and gather the exit status of its child processes.

Zombie Process
In Linux, a child process that has finished execution but whose parent process has not yet received its exit status is referred to as a "zombie process," also known as a "defunct" process. When a child process terminates, it remains as a zombie process in the process table until the parent process collects its exit status using the wait(), waitpid(), or SIGCHLD signals.
Although a zombie process doesn't use any system resources like CPU or memory, it does take up space in the process table and has the power to stop the creation of new processes if the system has reached the maximum number of processes it can support.
When using the top or ps commands, look for processes with a "Z" state in the process status column to identify zombie processes. Zombies processes are typically not a cause for concern because they are harmless; however, if a parent process crashes or exits abruptly without thoroughly cleaning up its child processes, the child processes may turn into zombies, which, if not handled properly, could result in a buildup of zombie processes.

The best practice for a parent process to properly close down its child processes when it exits, whether by using the wait() or waitpid() system calls or the SIGCHLD signal. This will help prevent zombie processes. When the SIGCHLD signal is periodically sent to the parent process, which causes it to wait for and gather the exit status of its child processes, the init process causes the parent process to wait for and clean up zombie processes.

Daemon Process
In Linux, a daemon process is a background process that runs without any user input and typically handles system-related duties. Daemon processes typically run continuously and are launched at system startup, carrying out operations like logging, scheduling, and managing network services.
Daemon processes do not directly interact with users and do not have a controlling terminal. They are typically started by the init process (PID 1) during system startup, and their settings are managed by configuration files and tools that run at the system level.
Daemon processes typically function without a terminal or display because they are made to run in the background. System-level utilities like systemctl or service can be used to start, stop, and restart them. The ps command or the top command can be used to check on their status.
The httpd daemon, which manages requests to the web server, the sshd daemon, which manages SSH connections, and the cron daemon, which manages scheduled tasks are a few examples of daemon processes.
Daemon processes are crucial for maintaining a Linux system's smooth operation because they offer crucial services and carry out background tasks required for the system to operate properly.


[*Linux process overview*](https://dev.to/rejoice/linux-process-management-a-deep-dive-2lmk)
[*What is Zombie Process in Linux?*](**https://www.scaler.com/topics/zombie-process-in-linux/**)




