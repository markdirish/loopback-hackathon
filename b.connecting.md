# Connecting to our IBM i Profiles through SSH

**If you have already connected with SSH, feel free to move on to the next section: [Running our Commands with Bash](c.bash.md)**

For this lab, we will be using a system provided by COMMON. I will provide you with the IP address and login instructions.

When we connect to our IBM i systems to develop open-source software, or run our open-source software, or simply interact with the IFS, we are going to do it in a way that may feel foreign to you: An SSH connection and a "normal" shell like `bash`.

What is SSH? SSH stands for "Secure SHell", and it is a protocol for secure communication between systems. We will be using SSH to create secure connections with our system to run commands in a `bash` shell session. SSH and SSH keys are also used for other uses, like securely pushing your code to GitHub from the command line, or syncing files between systems.

Now, I know what you are thinking: "Why do I need to use SSH + `bash`? I have a 5250 emulator that works great, and I can just use QSH!" Unfortunately, the 5250 environment and QSH are not "normal", and open-source software developers are targeting other configurations (a "modern" terminal running `bash`). Furthermore, the framework we will be using today, LoopBack, _will not work with QP2TERM or QShell_. We will make extensive use of the LoopBack CLI (command line interface), and it expects that you are running in a "normal" terminal. If you try to use it with QSH, it will just hang!

![LoopBack CLI doesn't work with QSH](assets/b.lb4qsh.png)

There is also a hidden bonus of doing things this way: You will be able to hire any developer who has worked on open-source software on Linux (or other terminal-heavy Unix systems), and be able to get them productive on IBM i from day 1. In fact, there are stories of new developers doing open-source work with SSH + `bash` on IBM i for months before realizing they weren't on a Linux system!

If you want to know more about why SSH should your preferred way of connecting to IBM i for doing PASE/open-source work, check out this blog from the wizard of IBM i open-source, Jesse Gorzinski: https://techchannel.com/Trends/08/2017/embrace-ssh

Unfortunately, the instructions for creating an SSH connection are different depending on which operating system you are running.

### **Mac and Linux**

For Unix systems (Linux, Mac, etc.), you probably have OpenSSH (or something similar) already installed. You should be able to connect to the system simply by opening the terminal on your system and running a command like:

```
ssh username@system
```

When you run the command, you will prompted for that password. When you enter your password correctly, you will be logged onto the IBM i system.

### **Windows**

For Windows, logging on using SSH is a little trickier. If you have a shell already set up, you can use what you know. If you have Access Client Solutions (ACS) installed, you can try creating a configuration for the lab system and running `SSH Terminal` in ACS and see what happens.

If you have a modern version of Windows, you could try the official Microsoft instructions here: https://learn.microsoft.com/en-us/windows/terminal/tutorials/ssh

If none of that applies, the most common solution is to download PuTTY: https://www.chiark.greenend.org.uk/~sgtatham/putty/

PuTTY is an implementation of SSH for Windows, and will allow you to enter a system name and configure a user name and password.

---

Now, if you log onto the system, you should be placed in your home directory (something like `/home/<userid>/`). You will also be given `bash` as your default shell. What is bash? Read on!

---
Next: [Running our Commands with Bash](c.bash.md)