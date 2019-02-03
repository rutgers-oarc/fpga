# Introduction to VNC

Some of the useful tools on fpga1 require a graphical interface, but X11 forwarding is normally very slow from a remote server such as fpga1. TurboVNC, on the other hand, is very fast. There is now gnome Linux desktop installed on the fpga1 machine, with the VNC server that's installed systemwide, so that any user of fpga1 can use the vncserver. You start the vncserver on fpga1, and the server forwards x-packets in an optimized and compressed form through ssh tunneling (with one hop through amarel) to your local machine and displays things like firefox (for viewing reports) or SDK tools, from the remote location. You need two things for that to happen: 

- you need to start a vncviewer on your local machine (and of course, install a vncviewer first) so that packets can get displayed on your local machine
- you need to do ssh tunneling from your local machine to fpga1 so that the packets can be forwarded from fpga1 to your local machine

## Install VNC client on your local machine

As root on your laptop (perform once):
```
sudo yum install tigervnc vinagre
```

## Tunnel to fpga1

ssh tunneling to fpga1 (`-A` for using your ssh keys, so no passwords necessary) means forwarding all packets through port 5091. On fpga1 you will start vnc server at this (default) port, so all x-packets will get sent through this port through to amarel and then to your laptop.

```
ssh -A -L 5901:localhost:5901 kp807@amarel.hpc.rutgers.edu  #use your own netid
ssh -A -L 5901:localhost:5901 fpga1
```

NOTE: If multiple people are using vncserver, each person will use a different port, and you have to replace 5901 with the port that your instance of vncserver is using. To see which one it is, look at the log file created when you start vncserver and find the port number.

## Start vncserver on fpga1

On fpga1 execute this command in a terminal (I'm pasting all my input and output):

```
#start vncserver
[kp807@fpga1 ~]$ vncserver

Desktop 'TurboVNC: fpga1:1 (kp807)' started on display fpga1:1

Starting applications specified in /home/kp807/.vnc/xstartup.turbovnc
Log file is /home/kp807/.vnc/fpga1:1.log
```

Go the the next section of the instruction (I'm putting the following information here because it belongs here logically, though not temporally): 

```
#after you are done with the session, to close the vncserver
[kp807@fpga1 ~]$ vncserver -kill :1

#if you have more than one session, list the sessions:
[kp807@fpga1 ~]$        vncserver -list
TurboVNC sessions:
X DISPLAY #     PROCESS ID
:1              311590
```

NOTE: the first time you start the vncserver, it will ask you to set up a password for your desktop (which you type in in the vnc client on your laptop when you open vncviewer) - please remember this password!

NOTE: same note for the possibly differing port than 5901 as before - look at the log file. It's named something like `.vnc/fpga1:1.log` in your home directory.

## Open VNC viewer on laptop

On your local machine, after installing vncviewer, run the following:
```
vncviewer &
```
and in the display type in `:1` (displays start from 0 upwards, and your `display:0` is your laptop). Type in the password that you set up when starting the vncserver. This need not be your rutgers netid password. 

This should be all - complicated enough!
