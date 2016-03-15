#Ansible create AWS EC2 instances in VPC, apply ELB, RDS (mysql) and run Django-CMS base app (use: nginx, gunicorn, supervisord)

##QUICK START:
##REQUIREMENTS:
####Project tested with:
```
Ansible 1.9.2 (also worked with Ansible 2.0.1.0 - just ignore DEPRECATION WARNING)
python boto module
host: Ubuntu 15.10 wily 4.2.0-23-generic x86_64
```
##QUICK START:

#### Install Ansible
```
sudo apt-get -y install software-properties-common
sudo apt-add-repository -y ppa:ansible/ansible
sudo apt-get update
sudo apt-get -y install ansible
```
#### Install python boto module
```
sudo apt-get -y install python-pip
sudo pip install boto
```
####Config Amazon AWS cloud service from AWS console:
```
https://console.aws.amazon.com/iam/home

select "Users" in left hand menu
create new user and download "credentials.csv"
select "Policies" in left hand menu
create administrator policy from amazons existing policies
attach to your user to "AdministratorAccess" policy
```

#Create and provision AWS EC2 instances with Ansible

####Add the following to your local users ~/.profile file
```
export AWS_ACCESS_KEY_ID="XXXXXXXXXXXXXXXX"             #Use 'Access Key Id' from credentials.csv
export AWS_SECRET_ACCESS_KEY="YYYYYYYYYYYYYYYYYYYYY"    #Use 'Secret Access Key' from credentials.csv
```
####To make the variables available in the current shell session run:
```
source ~/.profile
```

####Provision AWS instances with Ansible:
```
ansible-playbook -i hosts aws_go.yml
ansible-playbook -i ec2.py app_go.yml --limit tag_Name_test*
```
####Get more info:
#####[AWS Region and Availability Zone Concepts](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-regions-availability-zones)
#####[Amazon EC2 Security Groups](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html)
#####[Elastic Load Balancing](http://docs.aws.amazon.com/ElasticLoadBalancing/latest/DeveloperGuide/elb-getting-started.html)
#####[Amazon EC2 Key Pairs](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)
#####[Ansible AWS Dynamic Inventory](http://docs.ansible.com/ansible/intro_dynamic_inventory.html#example-aws-ec2-external-inventory-script)
#####[Boto config](http://docs.pythonboto.org/en/latest/boto_config_tut.html)
#####[Logrotate](https://support.rackspace.com/how-to/understanding-logrotate-utility/)

####Was helpfull for Debuging...
```
ansible all -m ping -i ./inventory/ec2.py
./ec2.py --refresh-cache
./ec2.py --list
ansible-playbook ansible/aws_ansible.yml -vvvv -i ansible/inventory/ec2.py --limit tag_Name_test*

sudo apt-get install awscli
aws ec2 describe-key-pairs --region=xxxxx
```
