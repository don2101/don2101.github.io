# EC2 생성후 실행 스크립트

```
#!/bin/sh
yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
yum -y install httpd php mysql php-mysql
chkconfig httpd on
service httpd start
if [ ! -f /var/www/html/ec2-linux-web.tar.gz ]; then
   cd /var/www/html
   wget https://for-distribution-cwk.s3-ap-northeast-2.amazonaws.com/ec2-linux-web.tar.gz
   tar xvfz ec2-linux-web.tar.gz
fi
yum -y update
```

