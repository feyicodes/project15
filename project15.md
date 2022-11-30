## Implementing AWS Cloud Solution For 2 Company Websites Using Reverse Proxy Technology

This projects expands the knowledge scope cloud exploration by looking at some concepts and tools available on AWS and other cloud providers. The project strenghtens my knowledge in the manual solutions to Clouod architecture, making the automation process more seamless. 
The project is based on a cloud architecture design that provides resilience to server failures, flexibility in managing traffic and cost effective.

![](images/img13.png)

I created a sub-account within my root account and an organizational unit named **Dev**. I added the sub account to the Dev organizational unit.

![](images/img2.png)

I created a hosted zone in AWS and mapped it to my registered domain **askogun.com**.

![](images/img3.png)

![](images/img5.png)

 I created a Virual Private Cloud with the appropriate subnet sufficient to cater for all instances that would be launched within. I created subnets as shown in the architecture and 2 different route tables associated with public subnet and private subnet. I created an internet gateway and edited aroute in the public route table, associating it with the internet gateway. 

![](images/img11.png)

![](images/img6.png)

![](images/img7.png)

![](images/img10.png)

I created 3 elastic IPs, assigned one to the Nat Gateway and the other are to be assigned to Bastion host.

![](images/img8.png)

![](images/img3.png)

![](images/img3.png)

![](images/img3.png)



I created 5 security groups catering for various infrastructure categories:
 * Nginx Servers
 * Bastion Servers
 * Application Load Balancer
 * Web servers
 * Data Layer.

![](images/img12.png)


 Based on the architecture, I would need to setup and configure the following resources in the VPC:
 EC2 INstances, Launch Templates, Target Groups, Autoscaling Groups, TLS Certificates and Application Load Balancers.

 I created an instance each of the Webserver, Nginx Server and Bastion Host server and carried out the necessary comfigurations to make it a suitable template for the architecture. 

The following code was used on all 3 instances for installing some of the necessary softwares namely: python, ntp, net-tools, vim, wget, telnet, epel-release, htop.
```
    yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

    yum install -y dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm

    yum install wget vim python3 telnet htop git mysql net-tools chrony -y

    systemctl start chronyd

    systemctl enable chronyd
```
The following code was used to configure the seleniux policies for the webservers and nginx servers
```
    setsebool -P httpd_can_network_connect=1
    setsebool -P httpd_can_network_connect_db=1
    setsebool -P httpd_execmem=1
    setsebool -P httpd_use_nfs 1
```

I also configured the utilities for mounting targets on Elastic File System for webserver and nginx server.
```
    git clone https://github.com/aws/efs-utils

    cd efs-utils

    yum install -y make

    yum install -y rpm-build

    make rpm 

    yum install -y  ./build/amazon-efs-utils*rpm
```

I configured the self signed certificate for the nginx instance:

```
    sudo mkdir /etc/ssl/private

    sudo chmod 700 /etc/ssl/private

    openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/ACS.key -out /etc/ssl/certs/ACS.crt

    sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
```







