# VNC server from a Windows machine

The steps to connect to turbovnc on fpga1 are similar as from a linux machine, but there are also important differences. 
So, please read the instructions for the linux machine first to understand tunneling. 

## Install: 

- TurboVNC client - download from https://www.turbovnc.org/ for windows
- putty - this is an ssh client - allows you to ssh into amarel, store ssh keys, store tunneling defaults etc

## Connect to fpga1

- In putty, save a session called "amarel" (or something you will remember), with the following defaults: 
 * the server name will be `amarel.hpc.rutgers.edu`
 * in the putty options, you find `Connection -> SSH -> Tunnels` and enter the port you want forwarded from amarel - this is 5902 for
 turbovnc. See [this screenshot](putty_configuration.png).
 * optionally add your authentication details if you don't want to type in your password all the time when connecting to amarel
 * save the connection details - this will enable you to do this once and forget it. 
- once putty connects to amarel, you need to tunnel the same port to fpga1. Now you are in the linux world (both amarel and fpga1 are linux)
so use the same command as in the other instructions: 
 ```
 [kp807@amarel2 ~]$ ssh -L 5902:127.0.0.1:5902 fpga1
```

## VNC tips

- if you forget your vnc password, you can regenerate it by going to fpga1 and issuing `vncpasswd` command
