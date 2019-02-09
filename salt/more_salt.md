
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



