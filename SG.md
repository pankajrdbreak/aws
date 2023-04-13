## Python script to add inbound rules with differnt port and same IP to multiple security groups
#### 1.  I have multiple security groups to which I want to add two inbound rules with two IP's & different ports.
#### 2.  I will create csv which contains IP,Description,SG ID,Type tcp/udp,Port.
#### 3.  Script will run in for loop and add rule to every sg mention in csv.


```console
import boto3
import csv
import sys
csv_file = "/path/to/csv/file/Sheet1.csv"
ec2 = boto3.resource('ec2')
f = open(csv_file)
csv_f = csv.reader(f)
for row in csv_f:
    try:
        cidr = row[0] + "/32"
        description = row[1]
        security_group_id = row[2]
        port = row[3]
        protocol = row[4]
        security_group = ec2.SecurityGroup(security_group_id)
        security_group.authorize_ingress(
        DryRun=False,
        IpPermissions=[
            {
                'FromPort': int(port),
                'ToPort': int(port),
                'IpProtocol': protocol,
                'IpRanges': [
                    {
                        'CidrIp': cidr,
                        'Description': description
                    },
                ]
            }
        ]
    )
    except Exception as err:
        print(f"Unexpected {err=}, {type(err)=}")
    
    
```
