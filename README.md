# diffengine-ansible
Diff Engine Installer

## Install Diffengine on AWS EC2

Using this playbook will cost you money. Depending on how long your leave your
ec2 running this will be charged to your AWS account. This has been tested on
Mac OSX and Ubuntu xenial.

### Prerequisites

You will need to install on  [Ansible](https://ansible.com) using. (using the OSX package manager `homebrew` did not work)

```
pip install -U ansible
```

and the Python's boto package with

```
pip install -U boto
```

### Set up AWS

Before running this playbook do the following to prepare

* create AWS access and security key  - see [Amazon's
  docs](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSGettingStartedGuide/AWSCredentials.html)

The values generated will be copied to be used later.

### Set up your host

Generate an ssh key that will connect to your AWS instance with

```
ssh-keygen -b 2048 -t rsa -C "awsdocnow" -f ~/.ssh/awsdocnow
```

Make a copy of `examples_aws_variables.yml` with

```
cp examples_aws_variables.yml aws_variables.yml
```

Copy your access and security key in the `aws_variables.yml` file. Make sure your AWS region matches the region you logged into and change it accordingly. If you want a beefier size machine change that also.

### Run the playbook

Add your generated key to your session with

```
ssh-add ~/.ssh/awsdocnow
```

If you run

```
ssh-add -l
```

It will list your newly generated key.

You are now ready to install Diffengine at an AWS endpoint with

```
ansible-playbook -vvvv -i inventory/ec2.py install_diffengine.yml -u ubuntu -b
```

We use the verbose so it lists the AWS ip you will be logging into upon completion which will look *similar* to this.

```
META: ran handlers
META: ran handlers

PLAY RECAP **********************************************************************************************************************************************************************************************************
1.2.3.4              : ok=7    changed=6    unreachable=0    failed=0
localhost                  : ok=9    changed=3    unreachable=0    failed=0
```

You can the ssh to your VM and run `diffengine` with

```
ssh ubuntu@1.2.3.4
ubuntu@ip-172-31-26-233:~$ diffengine
What RSS/Atom feed would you like to monitor?
