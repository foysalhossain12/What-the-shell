# What-the-shell

-------------- Improving ----------------------

## Netcat Shell Stabilisation :

### 👀 Technique 1: Python

The first technique we'll be discussing is applicable only to Linux boxes, as they will nearly always have Python installed by default. This is a three stage process:

The first thing to do is use
           
           python -c 'import pty;pty.spawn("/bin/bash")'
which uses Python to spawn a better featured bash shell; note that some targets may need the version of Python specified. If this is the case, replace python with python2 or python3 as required. At this point our shell will look a bit prettier, but we still won't be able to use tab autocomplete or the arrow keys, and Ctrl + C will still kill the shell.

Step two is:
             
            export TERM=xterm 
-- this will give us access to term commands such as clear.

Finally (and most importantly):

               using Ctrl + Z 
we will background the shell using Ctrl + Z. Back in our own terminal we use stty raw -echo; fg. This does two things: first, it turns off our own terminal echo (which gives us access to tab autocompletes, the arrow keys, and Ctrl + C to kill processes). It then foregrounds the shell, thus completing the process.

The full technique can be seen here:

![image](https://user-images.githubusercontent.com/55437834/122268164-8e0a0080-cefd-11eb-8ff4-4e60b61a7943.png)

### 🔥Technique 2: rlwrap

rlwrap is a program which, in simple terms, gives us access to history, tab autocompletion and the arrow keys immediately upon receiving a shell; however, some manual stabilisation must still be utilised if you want to be able to use Ctrl + C inside the shell. rlwrap is not installed by default on Kali, so first install it with sudo apt install rlwrap.

To use rlwrap, we invoke a slightly different listener:

            rlwrap nc -lvnp <port>

Prepending our netcat listener with "rlwrap" gives us a much more fully featured shell. This technique is particularly useful when dealing with Windows shells, which are otherwise notoriously difficult to stabilise. When dealing with a Linux target, it's possible to completely stabilise, by using the same trick as in step three of the previous technique: background the shell with Ctrl + Z, then use stty raw -echo; fg to stabilise and re-enter the shell.
