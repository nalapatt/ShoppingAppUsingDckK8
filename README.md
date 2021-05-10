# ShoppingAppUsingDckK8


This app is created to enhance the shopping experience

# 1st step
-  Create a load balancer in AWS
- Create an EC2 instance and associate an elastic IP address and load balancer to it 

# 2nd step
- Install Ansible:
sudo apt-get install software-properties-common
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible
ssh-keygen -t rsa ( just press enter for all the questions) ( it will create an ssh private key is id_rsa and a public key in id_rsa.pub) 
cat .ssh/id_rsa.pub >> .ssh/authorized_keys ( to copy the public keys into the authorized_keys file)

# 3rd step
- Install python and boto
sudo apt-get update

sudo apt-get upgrade
sudo apt-get install python3-pip

sudo apt-get install python-setuptools
sudo apt-get install python-pip
sudo pip install virtualenv

Then create the virtual env: 
virtualenv ansible_vEnv

Activate the virtual env: 
source ansible_vEnv/bin/activate

pip3 install boto
pip install boto
pip install boto3

# 4th step
- Create the ec2 instances using ansible playbook

Create User and generate access key
Create a user - give them full adminstrartor access and then generate and download the secret key
Install aws cli and configure with secret access key 
sudo apt  install awscli
aws configure 
AWS Access Key ID [None]: 
AWS Secret Access Key [None]: 
Default region name [None]: us-east-1
Default output format [None]:( Here give the secret access key from the csv file that was downloaded,next the access id, next us-east-1e, then enter for next selection)
now you will be able to see if
cd ~/.aws
ls -ltr
cat credentials
cat config
there will be two files credentials and config which contains the key and region
Connect to the ec2 instance
aws ec2 describe-instances
you should be able to see the json output
Go to the path where ansible is 
cd /etc/ansible/
ls - ltr
Play the ansible playbook
ansible-playbook ec2instanceansible.yml

# 5th step
- Install Docker
sudo apt-get update
sudo apt install docker.io
docker --version
- Install  kubernetes
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl


