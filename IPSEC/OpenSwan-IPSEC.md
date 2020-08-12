# VPN test on AWS using OpenSwan

| Region    | VPC-Name | CIDR          | Inst1 | Inst2 | etc1 | etc2 |
| --------- | -------- | ------------- | ----- | ----- | ---- | ---- |
| Seoul     | Seoul    | 10.0.0.0/16   |       |       |      |      |
| Tokyo     | Tokyo    | 20.0.0.0/16   |       |       |      |      |
| Singapore | Singa    | 30.100.0.0/16 |       |       |      |      |


**Create Seoul VPC 10.0.0.0/16**  
<kbd> ![GitHub Logo](oracle-to-s3-datalake-images/1.png) </kbd>

**Create EC2 Instance on Seoul VPC, One in Public, Another one in Private**  

***OpenSwan EC2 in Public Subnet*** 

Security Group - Seoul-OpenSwan

| Protocol | Source-Name |
| -------- | ----------- |
| SSH      | 0.0.0.0/0   |
| ALL ICMP | 0.0.0.0/0   |

***adf***


**

**








<kbd> ![GitHub Logo](oracle-to-s3-datalake-images/24.png) </kbd>