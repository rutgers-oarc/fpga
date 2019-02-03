# Introduction to VNC

X11 forwarding is normally very slow. TurboVNC is very fast. There is now gnome Linux desktop installed on fpga1 machine, with the VNC server that's installed systemwide and any user of fpga1 can use. You start the vncserverson fpga1, and it forwards x-packets in an optimized and compressed form through tunneling (with one hop through amarel) to your local machine. You need two things for that to happen: 

- you need to start a vncviewer (and of course, install a vncviewer first) so that packets can get displayed on your local machine
- you need to do ssh tunneling so that the packets can be forwarded from fpga1 to your local machine

## Install VNC client on your local machine

As root on your laptop (perform once):
```
sudo yum install tigervnc vinagre
```

## Tunnel to fpga1

Tunneling to fpga1 (`-A` for using your ssh keys, so no passwords necessary) means forwarding all packets through port 5091. On fpga1 you will start vnc server at this port, so all x-packets will get sent through this port through to amarel and then to your laptop.

```
ssh -A -L 5901:localhost:5901 kp807@amarel.hpc.rutgers.edu
ssh -A -L 5901:localhost:5901 fpga1
```

NOTE: If multiple people are using vncserver, each person will use a different port, and you have to replace 5901 with the port that your instance of vncserver is using. To see which one it is, look at the log file created when you start vncserver and find the port number.

## Start vncserver on fpga1

On fpga1 execute this command in a terminal:

```
#start vncserver
[kp807@fpga1 ~]$ vncserver

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

on laptop, after installing vncviewer, run the following:
```
vncviewer &
```
and in the display type in `:1` (displays start from 0 upwards, and your `display:0` is your laptop).

This should be all - complicated enough!
