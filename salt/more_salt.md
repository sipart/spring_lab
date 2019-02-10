Initial file structure of `/srv/pillar`:


```
pfne@ubuntu1804-pfne:~$ tree
.
├── srv
│   └── pillar
│       ├── proxy-ce1.sls
│       ├── proxy-ce2.sls
│       └── top.sls
```
Contents of the top.sls file - this file maps the proxy minion to each device:
```
base:
  'ce1':
    - proxy-ce1
  'ce2':
    - proxy-ce2
```
Contents of the two proxy .sls files:
```
proxy:
  proxytype: junos
  host: 10.132.0.221
  username: lab
  password: lab123
  port: 830
```

```
proxy:
  proxytype: junos
  host: 10.132.0.222
  username: lab
  password: lab123
  port: 830
```
Testing the minion/proxy minion connectivity from the salt master:
```
pfne@ubuntu1804-pfne:/srv$ sudo salt '*' test.ping
cumulus:
    True
ce2:
    True
ce1:
    True
```
Testing a salt command from the master and the response back - note the `'os_family:junos'` limiter so the command is only ran against Junos devices:

```
pfne@ubuntu1804-pfne:/srv$ sudo salt -G 'os_family:junos' junos.cli "show interfaces ge-0/0/3 terse"                                     [sudo] password for pfne:
ce2:
    ----------
    message:

        Interface               Admin Link Proto    Local                 Remote
        ge-0/0/3                up    up
        ge-0/0/3.0              up    up   inet     10.132.0.222/20
    out:
        True
ce1:
    ----------
    message:

        Interface               Admin Link Proto    Local                 Remote
        ge-0/0/3                up    up
        ge-0/0/3.0              up    up   inet     10.132.0.221/20
    out:
        True
```



