# Overlan


Python Remote Access Tools

For full documentation visit [Overlan](http://141.94.203.12:8000).


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


## Install

Unix:

	pip install overlan
	
	# or

	python3 -m pip install overlan
	

Windows:

	py -m pip install overlan
	


Git:


	git clone https://github.com/YannBouyeron/overlan

	cd overlan

	python3 setup.py install
	


## Commands Line Interface


#### Listen

Listen on localhost port 4444
	
	overlan -l 4444

Listen on localhost port 4444 password protected
	
	overlan -l 4444 -k mysecret

Listen on port 4444 and remote forward to overlan server port 8888
	
	overlan -l 4444 -R 8888

#### Connect

Connect to localhost port 4444
	
	overlan localhost 4444

Connect to localhost port 4444 password protected 
	
	overlan localhost 4444 -k mysecret

Connect to overlan server port 4444
	
	overlan overlan 4444

#### Port forwarding

###### Overlan

Remote forward localhost 4444 to overlan server port 8888

	overlan 4444 -R 8888

Local forward overlan server port 8888 to localhost 4444

	overlan 8888 -L 4444

###### SSH

Remote forward localhost 4444 to overlan server port 8888 

	ssh -N -R 8888:localhost:4444 overlan@141.94.203.12 -p 22

Local forward localhost 4444 to overlan server port 8888 

	ssh -N -L 4444:localhost:8888 overlan@141.94.203.12 -p 22

#### Bind Shell

Unix

- `overlan -l 4444 -e /bin/bash` - BindShell listen on localhost port 4444.

- `overlan -l 4444 -e /bin/bash -R 8888` - BindShell listen on port 4444 and remote forward to overlan server port 8888.
	
- `overlan -l 4444 -e /bin/bash -k mypassowrd` - Password protected BindShell listen on localhost 4444.

- `overlan -l 4444 -e /bin/bash -k mypassword -R 8888` - Password protected BindShell listen on port 4444 and remote forward to overlan server port 8888.

Windows

- `overlan -l 4444 -e cmd.exe` - BindShell listen on localhost port 4444.

- `overlan -l 4444 -e cmd.exe -R 8888` - BindShell listen on port 4444 and remote forward to overlan server port 8888.
	
- `overlan -l 4444 -e cmd.exe -k mypassowrd` - Password protected BindShell listen on localhost port 4444.

- `overlan -l 4444 -e cmd.exe -k mypassword -R 8888` - Password protected BindShell listen on port 4444 and remote forward to overlan server port 8888.

#### Reverse Shell

Unix

- `overlan localhost 4444 -e /bin/bash` - ReverseShell connect to listener on localhost port 4444.

- `overlan overlan 4444 -e /bin/bash` - ReverseShell connect to listener on overlan server port 4444.

- `overlan localhost 4444 -e /bin/bash -k mypassowrd` - Password protected ReverseShell connect to localhost port 4444.

- `overlan overlan 4444 -e /bin/bash -k mypassowrd` - Password protected ReverseShell connect to overlan server port 4444.

Windows

- `overlan localhost 4444 -e cmd.exe` - ReverseShell connect to listener on localhost port 4444.

- `overlan overlan 4444 -e cmd.exe` - ReverseShell connect to listener on overlan server port 4444.

- `overlan localhost 4444 -e cmd.exe -k mypassowrd` - Password protected ReverseShell connect to localhost port 4444.

- `overlan overlan 4444 -e cmd.exe -k mypassowrd` - Password protected ReverseShell connect to overlan server port 4444.


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
                        Remote forward to remote_port
  -L LOCAL, --local LOCAL
                        Local forward from remote_port
  -k KEY, --key KEY     Password for AES derivation key
  -d DELAY, --delay DELAY
                        Timeout delay
```

## Use within python


Use overlan ssh server:


```
from overlan import Overlan

v = Overlan()

```

Specify another external ssh server:

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


