# ShoppingAppUsingDckK8


This app is created to enhance the shopping experience

1st step
Create an EC2 instance and associate an elastic IP address to it

2nd step
Ansible:
sudo apt-get install software-properties-common
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible
ssh-keygen -t rsa ( just press enter for all the questions) ( it will create an ssh private key is id_rsa and a public key in id_rsa.pub) 
cat .ssh/id_rsa.pub >> .ssh/authorized_keys ( to copy the public keys into the authorized_keys file)
sudo vi /etc/ansible/hosts ( to configure the hosts )
press i ( to get into insert mode)
at the end of the file enter 
webservers 
public ip address of the server VM on aws
Esc :wq to save and exit the editor
ansible –m ping webservers ( to check if you have connection to the servers )

3rd step
Play the ansible playbook
ansible-playbook ec2instanceansible.yml
