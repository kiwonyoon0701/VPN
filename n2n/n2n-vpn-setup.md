### server private-ip vpn-ip

| Node type  | Private IP   | VPN IP      | etc |
| ---------- | ------------ | ----------- | --- |
| VPN Server | 172.31.1.10  | 0           |     |
| client1    | 172.31.1.101 | 192.168.1.2 |     |
| xlarge     | 172.31.1.102 | 192.168.1.3 |     |
| xlarge     | 172.31.1.103 | 192.168.1.4 |     |

### n2n software install on server/client

```
ubuntu@ip-172-31-1-10:/home/ubuntu> sudo apt-get install n2n
```

```
ubuntu@ip-172-31-1-101:/home/ubuntu> sudo apt-get install n2n
ubuntu@ip-172-31-1-102:/home/ubuntu> sudo apt-get install n2n
ubuntu@ip-172-31-1-103:/home/ubuntu> sudo apt-get install n2n
```

### Start n2n VPN Server

```
ubuntu@ip-172-31-1-10:/home/ubuntu> sudo supernode -l 12000
08/Apr/2021 00:48:10 [supernode.c: 476] Supernode ready: listening on port 12000 [TCP/UDP]

ubuntu@ip-172-31-1-10:/home/ubuntu> sudo netstat -an |grep 12000
tcp        0      0 0.0.0.0:12000           0.0.0.0:*               LISTEN
udp        0      0 0.0.0.0:12000           0.0.0.0:*
```

### Connect VPN from Client1

```
ubuntu@ip-172-31-1-101:/home/ubuntu> sudo edge -c mynetwork -k mysecpass -a 192.168.1.2 -f -l 172.31.1.10:12000
192.168.1.2
08/Apr/2021 00:49:29 [     edge.c:1136] Using supernode 172.31.1.10:12000
08/Apr/2021 00:49:29 [tuntap_linux.c:  38] Interface edge0 has MAC 7A:E3:61:69:C0:B0

ubuntu@ip-172-31-1-101:/home/ubuntu> ip a s
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: ens5: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9001 qdisc mq state UP group default qlen 1000
    link/ether 02:db:ca:9f:f7:38 brd ff:ff:ff:ff:ff:ff
    inet 172.31.1.101/20 brd 172.31.15.255 scope global dynamic ens5
       valid_lft 3271sec preferred_lft 3271sec
    inet6 fe80::db:caff:fe9f:f738/64 scope link
       valid_lft forever preferred_lft forever
3: edge0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1400 qdisc fq_codel state UNKNOWN group default qlen 1000
    link/ether 7a:e3:61:69:c0:b0 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.2/24 brd 192.168.1.255 scope global edge0
       valid_lft forever preferred_lft forever
    inet6 fe80::78e3:61ff:fe69:c0b0/64 scope link
       valid_lft forever preferred_lft forever

```

### Connect VPN from Client2

```
ubuntu@ip-172-31-1-102:/home/ubuntu> sudo edge -c mynetwork -k mysecpass -a 192.168.1.3 -f -l 172.31.1.10:12000
192.168.1.3
08/Apr/2021 00:53:15 [     edge.c:1136] Using supernode 172.31.1.10:12000
08/Apr/2021 00:53:15 [tuntap_linux.c:  38] Interface edge0 has MAC 3A:4C:D3:2A:0B:64

ubuntu@ip-172-31-1-102:/home/ubuntu> ping -c 1 192.168.1.2
PING 192.168.1.2 (192.168.1.2) 56(84) bytes of data.
64 bytes from 192.168.1.2: icmp_seq=1 ttl=64 time=1.27 ms

--- 192.168.1.2 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 1.273/1.273/1.273/0.000 ms

ubuntu@ip-172-31-1-102:/home/ubuntu> traceroute 192.168.1.2
traceroute to 192.168.1.2 (192.168.1.2), 30 hops max, 60 byte packets
 1  ip-192-168-1-2.ap-northeast-2.compute.internal (192.168.1.2)  0.594 ms  0.586 ms  0.582 ms
```

### Connect VPN from Client3

```
ubuntu@ip-172-31-1-103:/home/ubuntu> sudo edge -c mynetwork -k mysecpass -a 192.168.1.4 -f -l 172.31.1.10:12000
192.168.1.4
08/Apr/2021 00:54:17 [     edge.c:1136] Using supernode 172.31.1.10:12000
08/Apr/2021 00:54:17 [tuntap_linux.c:  38] Interface edge0 has MAC 8E:54:21:6C:B2:2

ubuntu@ip-172-31-1-103:/home/ubuntu> ping -c 1 192.168.1.2
PING 192.168.1.2 (192.168.1.2) 56(84) bytes of data.
64 bytes from 192.168.1.2: icmp_seq=1 ttl=64 time=1.28 ms

--- 192.168.1.2 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 1.289/1.289/1.289/0.000 ms
```

### VPN Server log

```
ubuntu@ip-172-31-1-10:/home/ubuntu> sudo supernode -l 12000
08/Apr/2021 00:48:10 [supernode.c: 476] Supernode ready: listening on port 12000 [TCP/UDP]
08/Apr/2021 00:49:29 [supernode.c: 119] Registered new node [public_ip=(2)172.31.1.101:42899][private_ip=0.0.0.0:42899][mac=7A:E3:61:69:C0:B0][community=mynetwork]
08/Apr/2021 00:53:15 [supernode.c: 119] Registered new node [public_ip=(2)172.31.1.102:50229][private_ip=0.0.0.0:50229][mac=3A:4C:D3:2A:0B:64][community=mynetwork]
08/Apr/2021 00:54:17 [supernode.c: 119] Registered new node [public_ip=(2)172.31.1.103:54188][private_ip=0.0.0.0:54188][mac=8E:54:21:6C:B2:20][community=mynetwork]
```

### VPN Client netstat

```

ubuntu@ip-172-31-1-10:/home/ubuntu> netstat -an |grep 0.0.0.0
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:12000           0.0.0.0:*               LISTEN
udp        0      0 0.0.0.0:12000           0.0.0.0:*
udp        0      0 127.0.0.53:53           0.0.0.0:*
udp        0      0 172.31.1.10:68          0.0.0.0:*

ubuntu@ip-172-31-1-101:/home/ubuntu> netstat -an |grep 42899
udp        0      0 0.0.0.0:42899           0.0.0.0:*

ubuntu@ip-172-31-1-102:/home/ubuntu> netstat -an |grep 50229
udp        0      0 0.0.0.0:50229           0.0.0.0:*

ubuntu@ip-172-31-1-103:/home/ubuntu> netstat -an |grep 54188
udp        0      0 0.0.0.0:54188           0.0.0.0:*
```

# File Transfer Test

**Start Listen on client2**

```
ubuntu@ip-172-31-1-101:/home/ubuntu> sudo nc -l 5001 > /dev/null
```

**Send file from client3 to client2**

```
ubuntu@ip-172-31-1-102:/home/ubuntu> dd if=/dev/urandom of=./3gfile bs=1024k count=3000

ubuntu@ip-172-31-1-102:/home/ubuntu> cat ./3gfile |pv |nc 192.168.1.2 5001
1.08GiB 0:00:33 [33.5MiB/s] [

ubuntu@ip-172-31-1-102:/home/ubuntu> cat ./3gfile |pv |nc 192.168.1.2 5001
2.93GiB 0:01:29 [33.4MiB/s] [                                       <=>

```

**Establishing VPN connection through public IP**

```
ubuntu@ip-172-31-1-10:/home/ubuntu> sudo supernode -l 12000
ubuntu@ip-172-31-1-101:/home/ubuntu> sudo edge -c mynetwork -k mysecpass -a 192.168.1.2 -f -l 54.180.134.213:12000

ubuntu@ip-172-31-1-102:/home/ubuntu> sudo edge -c mynetwork -k mysecpass -a 192.168.1.3 -f -l 54.180.134.213:12000

08/Apr/2021 08:38:44 [supernode.c: 476] Supernode ready: listening on port 12000 [TCP/UDP]
08/Apr/2021 08:39:17 [supernode.c: 119] Registered new node [public_ip=(2)3.35.176.87:49473][private_ip=0.0.0.0:49473][mac=66:15:4A:2C:9C:F5][community=mynetwork]
```

**Route table**

```
ubuntu@ip-172-31-1-101:/home/ubuntu> route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         172.31.0.1      0.0.0.0         UG    100    0        0 ens5
172.31.0.0      0.0.0.0         255.255.240.0   U     0      0        0 ens5
172.31.0.1      0.0.0.0         255.255.255.255 UH    100    0        0 ens5
192.168.1.0     0.0.0.0         255.255.255.0   U     0      0        0 edge0
```
