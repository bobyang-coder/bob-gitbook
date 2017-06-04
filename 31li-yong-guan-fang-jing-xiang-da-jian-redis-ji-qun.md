`1、 docker pull redis`

`2、docker run -d --name redis-1 -p 7001:6379 redis`

`3、docker inspect redis-1 | grep IPA`

"SecondaryIPAddresses": null,  
            "IPAddress": "172.17.0.2",  
                    "IPAMConfig": null,  
                    "IPAddress": "172.17.0.2",

`4、启动slave`

`docker run -d --name redis-2 -p 7002:6379 redis redis-server --slaveof172.17.0.26379    
docker run -d --name redis-3 -p 7003:6379 redis redis-server --slaveof172.17.0.26379`

`5、验证：`

```
redis-cli -h 127.0.0.1 -p 7001
127.0.0.1:7001> get key
"hello"
127.0.0.1:7001> exit
redis-cli -h 127.0.0.1 -p 7002
127.0.0.1:7002> get key
"hello"
127.0.0.1:7002> exit
redis-cli -h 127.0.0.1 -p 7003
127.0.0.1:7003> get key
"hello"
127.0.0.1:7003> exit
```



