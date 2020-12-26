Reference site : https://prasaddomala.com/2020/04/02/aws-client-vpn-setup-private-access-across-aws-accounts-and-vpcs/

**Create Private Certificate**

```
kiwony@kiwonymac.com:/Users/kiwony/Documents/GitHub> git clone https://github.com/OpenVPN/easy-rsa.git

kiwony@kiwonymac.com:/Users/kiwony/Documents/GitHub/easy-rsa/easyrsa3> ./easyrsa init-pki

kiwony@kiwonymac.com:/Users/kiwony/Documents/GitHub/easy-rsa/easyrsa3> cd pki
%
kiwony@kiwonymac.com:/Users/kiwony/Documents/GitHub/easy-rsa/easyrsa3/pki> ll
total 32
drwxr-xr-x  7 kiwony  staff   224 Dec 26 09:19 ..
drwx------  2 kiwony  staff    64 Dec 26 09:19 private
drwx------  2 kiwony  staff    64 Dec 26 09:19 reqs
-rw-------  1 kiwony  staff  4616 Dec 26 09:19 openssl-easyrsa.cnf
-rw-------  1 kiwony  staff  4910 Dec 26 09:19 safessl-easyrsa.cnf
drwx------  6 kiwony  staff   192 Dec 26 09:19 .

kiwony@kiwonymac.com:/Users/kiwony/Documents/GitHub/easy-rsa/easyrsa3> ./easyrsa build-ca nopass
Using SSL: openssl LibreSSL 2.8.3
Generating RSA private key, 2048 bit long modulus
......................................................................+++
...................................................................................................................................+++
e is 65537 (0x10001)
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Common Name (eg: your user, host, or server name) [Easy-RSA CA]:clientvpndemo.com

CA creation complete and you may now import and sign cert requests.
Your new CA certificate file for publishing is at:
/Users/kiwony/Documents/GitHub/easy-rsa/easyrsa3/pki/ca.crt


kiwony@kiwonymac.com:/Users/kiwony/Documents/GitHub/easy-rsa/easyrsa3> ./easyrsa build-server-full clientvpndemo.com nopass
Using SSL: openssl LibreSSL 2.8.3
Generating a 2048 bit RSA private key
........+++
........................................................................................................................................................+++
writing new private key to '/Users/kiwony/Documents/GitHub/easy-rsa/easyrsa3/pki/easy-rsa-39622.f2x4K1/tmp.pQOZ52'
-----
Using configuration from /Users/kiwony/Documents/GitHub/easy-rsa/easyrsa3/pki/easy-rsa-39622.f2x4K1/tmp.0yLMzm
Check that the request matches the signature
Signature ok
The Subject's Distinguished Name is as follows
commonName            :ASN.1 12:'clientvpndemo.com'
Certificate is to be certified until Mar 31 00:22:06 2023 GMT (825 days)

Write out database with 1 new entries
Data Base Updated

kiwony@kiwonymac.com:/Users/kiwony/Documents/GitHub/easy-rsa/easyrsa3> ./easyrsa build-client-full ikoo.clientvpndemo.com nopass
Using SSL: openssl LibreSSL 2.8.3
Generating a 2048 bit RSA private key
.............................................................................................................+++
........................................................+++
writing new private key to '/Users/kiwony/Documents/GitHub/easy-rsa/easyrsa3/pki/easy-rsa-39779.TSPE98/tmp.wQlXNM'
-----
Using configuration from /Users/kiwony/Documents/GitHub/easy-rsa/easyrsa3/pki/easy-rsa-39779.TSPE98/tmp.wjtejd
Check that the request matches the signature
Signature ok
The Subject's Distinguished Name is as follows
commonName            :ASN.1 12:'ikoo.clientvpndemo.com'
Certificate is to be certified until Mar 31 00:22:49 2023 GMT (825 days)

Write out database with 1 new entries
Data Base Updated

kiwony@kiwonymac.com:/Users/kiwony/Documents/GitHub/easy-rsa/easyrsa3> mkdir acm
kiwony@kiwonymac.com:/Users/kiwony/Documents/GitHub/easy-rsa/easyrsa3> cp cp pki/ca.crt acm; cp pki/issued/clientvpndemo.com.crt acm; cp pki/issued/ikoo.clientvpndemo.com.crt acm; cp pki/private/clientvpndemo.com.key acm; cp pki/private/ikoo.clientvpndemo.com.key acm



kiwony@kiwonymac.com:/Users/kiwony/Documents/GitHub/easy-rsa/easyrsa3/acm> awsaws acm import-certificate --certificate fileb://clientvpndemo.com.crt --private-key fileb://clientvpndemo.com.key --certificate-chain fileb://ca.crt --region ap-northeast-1
%
kiwony@kiwonymac.com:/Users/kiwony/Documents/GitHub/easy-rsa/easyrsa3/acm> awsaws acm import-certificate --certificate fileb://ikoo.clientvpndemo.com.crt --private-key fileb://ikoo.clientvpndemo.com.key --certificate-chain fileb://ca.crt --region ap-northeast-1

```

<kbd> ![GitHub Logo](images/1.png) </kbd>



**Create VPC and attach DHCP Option Set**

<kbd> ![GitHub Logo](images/2.png) </kbd>

<kbd> ![GitHub Logo](images/3.png) </kbd>


**Create VPC and attach DHCP Option Set**
```

<kbd> ![GitHub Logo](images/1.png) </kbd>
```
**Create VPC and attach DHCP Option Set**
```

```
**Create VPC and attach DHCP Option Set**
```

<kbd> ![GitHub Logo](images/1.png) </kbd>
```
**Create VPC and attach DHCP Option Set**
```

<kbd> ![GitHub Logo](images/1.png) </kbd>
```
**Create VPC and attach DHCP Option Set**
```

<kbd> ![GitHub Logo](images/1.png) </kbd>
```
**Create VPC and attach DHCP Option Set**
```

<kbd> ![GitHub Logo](images/1.png) </kbd>
```
