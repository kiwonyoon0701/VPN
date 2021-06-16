### Create Certificate

```
kiwony@kiwonymac.com:/Users/kiwony/temp> git clone https://github.com/OpenVPN/easy-rsa.git
kiwony@kiwonymac.com:/Users/kiwony/temp> cd easy-rsa/easyrsa3
kiwony@kiwonymac.com:/Users/kiwony/temp/easy-rsa/easyrsa3>


kiwony@kiwonymac.com:/Users/kiwony/temp/easy-rsa/easyrsa3> ./easyrsa init-pki

init-pki complete; you may now create a CA or requests.
Your newly created PKI dir is: /Users/kiwony/temp/easy-rsa/easyrsa3/pki

kiwony@kiwonymac.com:/Users/kiwony/temp/easy-rsa/easyrsa3> ls -alrt
total 192
-rwxr-xr-x   1 kiwony  staff  76904 Jun 15 23:16 easyrsa
-rw-r--r--   1 kiwony  staff   4616 Jun 15 23:16 openssl-easyrsa.cnf
-rw-r--r--   1 kiwony  staff   8925 Jun 15 23:16 vars.example
drwxr-xr-x  10 kiwony  staff    320 Jun 15 23:16 x509-types
drwxr-xr-x  20 kiwony  staff    640 Jun 15 23:16 ..
drwxr-xr-x   7 kiwony  staff    224 Jun 15 23:16 .
drwx------   6 kiwony  staff    192 Jun 15 23:16 pki


kiwony@kiwonymac.com:/Users/kiwony/temp/easy-rsa/easyrsa3> ./easyrsa build-ca nopass
Using SSL: openssl LibreSSL 2.8.3
Generating RSA private key, 2048 bit long modulus
................................................+++
..............................................+++
e is 65537 (0x10001)
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Common Name (eg: your user, host, or server name) [Easy-RSA CA]:kiwony.com

CA creation complete and you may now import and sign cert requests.
Your new CA certificate file for publishing is at:
/Users/kiwony/temp/easy-rsa/easyrsa3/pki/ca.crt

kiwony@kiwonymac.com:/Users/kiwony/temp/easy-rsa/easyrsa3> ./easyrsa build-server-full server nopass
Using SSL: openssl LibreSSL 2.8.3
Generating a 2048 bit RSA private key
.....................................................................................+++
.....................................................+++
writing new private key to '/Users/kiwony/temp/easy-rsa/easyrsa3/pki/easy-rsa-56357.2KIn9F/tmp.7L6N2G'
-----
Using configuration from /Users/kiwony/temp/easy-rsa/easyrsa3/pki/easy-rsa-56357.2KIn9F/tmp.twZ54A
Check that the request matches the signature
Signature ok
The Subject's Distinguished Name is as follows
commonName            :ASN.1 12:'server'
Certificate is to be certified until Sep 18 14:18:25 2023 GMT (825 days)

Write out database with 1 new entrie
Data Base Updated

kiwony@kiwonymac.com:/Users/kiwony/temp/easy-rsa/easyrsa3> ./easyrsa build-server-full kiwony.com nopass
Using SSL: openssl LibreSSL 2.8.3
Generating a 2048 bit RSA private key
.........+++
.............+++
writing new private key to '/Users/kiwony/temp/easy-rsa/easyrsa3/pki/easy-rsa-56863.EYJLnd/tmp.e804UT'
-----
Using configuration from /Users/kiwony/temp/easy-rsa/easyrsa3/pki/easy-rsa-56863.EYJLnd/tmp.8jwKfx
Check that the request matches the signature
Signature ok
The Subject's Distinguished Name is as follows
commonName            :ASN.1 12:'kiwony.com'
Certificate is to be certified until Sep 18 14:20:29 2023 GMT (825 days)

Write out database with 1 new entries
Data Base Updated

kiwony@kiwonymac.com:/Users/kiwony/temp/easy-rsa/easyrsa3> ./easyrsa build-client-full user01.kiwony.com nopass

Using SSL: openssl LibreSSL 2.8.3
Generating a 2048 bit RSA private key
............................+++
...............+++
writing new private key to '/Users/kiwony/temp/easy-rsa/easyrsa3/pki/easy-rsa-57025.4ST2Rj/tmp.hmZQYG'
-----
Using configuration from /Users/kiwony/temp/easy-rsa/easyrsa3/pki/easy-rsa-57025.4ST2Rj/tmp.oxYD72
Check that the request matches the signature
Signature ok
The Subject's Distinguished Name is as follows
commonName            :ASN.1 12:'user01.kiwony.com'
Certificate is to be certified until Sep 18 14:21:09 2023 GMT (825 days)

Write out database with 1 new entries
Data Base Updated

kiwony@kiwonymac.com:/Users/kiwony/temp/easy-rsa/easyrsa3> mkdir acm
kiwony@kiwonymac.com:/Users/kiwony/temp/easy-rsa/easyrsa3> cp pki/ca.crt acm/
kiwony@kiwonymac.com:/Users/kiwony/temp/easy-rsa/easyrsa3> cp pki/issued/* ./acm/
kiwony@kiwonymac.com:/Users/kiwony/temp/easy-rsa/easyrsa3> cp pki/private/*kiwony* ./acm/

kiwony@kiwonymac.com:/Users/kiwony/temp/easy-rsa/easyrsa3> ls -alrt ./acm
total 56
drwxr-xr-x  8 kiwony  staff   256 Jun 15 23:21 ..
-rw-------  1 kiwony  staff  1168 Jun 15 23:21 ca.crt
-rw-------  1 kiwony  staff  4562 Jun 15 23:22 kiwony.com.crt
-rw-------  1 kiwony  staff  4453 Jun 15 23:22 user01.kiwony.com.crt
-rw-------  1 kiwony  staff  1708 Jun 15 23:23 kiwony.com.key
-rw-------  1 kiwony  staff  1704 Jun 15 23:23 user01.kiwony.com.key
drwxr-xr-x  7 kiwony  staff   224 Jun 15 23:23 .


kiwony@kiwonymac.com:/Users/kiwony/temp/easy-rsa/easyrsa3> cd acm
kiwony@kiwonymac.com:/Users/kiwony/temp/easy-rsa/easyrsa3/acm> aws acm import-certificate --certificate fileb://kiwony.com.crt --private-key fileb://kiwony.com.key --certificate-chain fileb://ca.crt --region us-west-1

kiwony@kiwonymac.com:/Users/kiwony/temp/easy-rsa/easyrsa3/acm> aws acm import-certificate --certificate fileb://user01.kiwony.com.crt --private-key fileb://user01.kiwony.com.key --certificate-chain fileb://ca.crt --region us-west-1



```

<kbd> ![GitHub Logo](images/1.png) </kbd>

### Create VPN Endpoint

<kbd> ![GitHub Logo](images/2.png) </kbd>

<kbd> ![GitHub Logo](images/3.png) </kbd>

<kbd> ![GitHub Logo](images/4.png) </kbd>

<kbd> ![GitHub Logo](images/5.png) </kbd>

**wait 5~10min to associate**

<kbd> ![GitHub Logo](images/6.png) </kbd>

<kbd> ![GitHub Logo](images/7.png) </kbd>

<kbd> ![GitHub Logo](images/8.png) </kbd>

<kbd> ![GitHub Logo](images/9.png) </kbd>

**Modify and add the line**

```
remote user01.cvpn-endpoint-0ac7db2bae8a8125d.prod.clientvpn.us-west-1.amazonaws.com 443
cert C:\\vpn-config\\user01.kiwony.com.crt
key C:\\vpn-config\\user01.kiwony.com.key

```

<kbd> ![GitHub Logo](images/10.png) </kbd>

<kbd> ![GitHub Logo](images/11.png) </kbd>

<kbd> ![GitHub Logo](images/12.png) </kbd>

<kbd> ![GitHub Logo](images/13.png) </kbd>

<kbd> ![GitHub Logo](images/14.png) </kbd>

<kbd> ![GitHub Logo](images/15.png) </kbd>

<kbd> ![GitHub Logo](images/16.png) </kbd>

<kbd> ![GitHub Logo](images/17.png) </kbd>

<kbd> ![GitHub Logo](images/18.png) </kbd>

<kbd> ![GitHub Logo](images/19.png) </kbd>

**Default gateway is still using lan gw, client can not access to internet, and can access to default VPC-EC2**

<kbd> ![GitHub Logo](images/20.png) </kbd>

**Delete current default gw and add new gw for VPN endpoint**

```
C:\WINDOWS\system32>route delete 0.0.0.0
 OK!

C:\WINDOWS\system32>route add -p 0.0.0.0 mask 0.0.0.0 20.0.1.1
 OK!

 C:\WINDOWS\system32>route print|more

IPv4 Route Table
===========================================================================
Active Routes:
Network Destination        Netmask          Gateway       Interface  Metric
          0.0.0.0          0.0.0.0         20.0.1.1         20.0.1.2      2
   18.144.131.213  255.255.255.255      192.168.0.1     192.168.0.10     35
```

<kbd> ![GitHub Logo](images/21.png) </kbd>
