---
title: "User mode, Kernel mode and Syscalls"
date: 2022-04-21T19:17:15+02:00
draft: true
---

Kernel is just another name for operating system (OS). Frequently when we talk about an operating system such as Microsoft Windows we think of the graphical user interface that we see when turning on the PC, but all that is not really the operating system (or kernel), but instead software running on top of the OS.

<!--more-->

The purpose of the operating system is to hide the complexity of the hardware and to manage and control access to that hardware. Hiding the complexity means that despite there being lots of different hard disk vendors and models the operating system hides all those differences from you and you can just access every hard disk the same way. Managing and controlling access ensures that for example one process does not move the hard disk write head while that head is in the middle of writing some data. It would be chaos!

The kernel therefore is what gets started first when booting up a computer and it reserves certain privileges (such as direct access to hardware) that it does not grant to other processes. Those other processes are called user processes. Note that these levels of permission have nothing to do with user permissions, which may for instance allow certain users on a computer to access some folders and other not.

Anything that the kernel itself does is called "kernel mode" and everything else is "user mode".

If a normal process (such as an image editor) wants to write data to disk it can't do it by themselves, but instead has to ask the kernel to do it. The kernel will make sure that the disk is not in use for a fraction of time and fulfills the request.

The kernel is also nothing more than a program that resides in memory (RAM) and needs the CPU to execute.

Picture of memory.

If the kernel is just another program in memory and I as a user can write a program, compile it and load it into memory, what stops me from writing a program that does exactly the same as the kernel?

Because the CPU will only execute certain instructions, such as those accessing hardware when the CPU is in "kernel mode". Any user program trying to execute such an instruction would be immediately terminated by the CPU. You might have seen this happening when (accidentally) accessing memory that does not belong to your process. The CPU knows that a process is only allowed to access certain areas of memory assigned to it. If it tries to access anything else it must be in "kernel mode". If it's in user mode it will be killed.

How is it possible for the CPU to be in kernel mode? There is a register PSW containing a bit. If this bit is 1 then the CPU is in kernel mode. If it's 0 then the CPU is in user mode.

One of the instructions that the CPU will NOT execute in user mode is modifying that "mode" bit.

How does a user process tell the kernel that it needs something if the user process can never switch the PSW register into kernel mode? That's where system calls come in.

A system call is just like a normal procedure (function) call. With the particularity that it will first set the PSW register (switching into kernel mode). And that the _function_ being called is defined in the kernel code.

How does this happen?

The user process first places the system call number it wants to execute into a certain CPU register. Then it executes a TRAP instruction. The TRAP instruction makes the CPU switch into kernel mode and start executing kernel code that will read the system call number and call the appropiate system call handler.

The system call handler will execute what is expected in an appropiate moment. It can also block if for example it needs to wait for hardware input (in which case the CPU will send the process to sleep until the hardware is ready). Finally the system call handler will switch the CPU back into user mode and return control to the user process.
