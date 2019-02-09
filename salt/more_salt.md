
```
pfne@ubuntu1804-pfne:/srv$ tree -A
.
└── pillar
    ├── proxy-ce1.sls
    ├── proxy-ce2.sls
    └── top.sls
```

```
base:
  'ce1':
    - proxy-ce1
  'ce2':
    - proxy-ce2
```

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

```
pfne@ubuntu1804-pfne:/srv$ sudo salt '*' test.ping
cumulus:
    True
ce2:
    True
ce1:
    True
```


```
pfne@ubuntu1804-pfne:/srv$ sudo salt -G 'os_family:junos' junos.cli "show interfaces ge-0/0/3 terse"                                                                                               [sudo] password for pfne:
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



