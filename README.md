# ShoppingAppUsingDckK8


This app is created to enhance the shopping experience

# Create an EC2 instance
-  Create a load balancer in AWS
- Create an EC2 instance and associate an elastic IP address and load balancer to it 

# Install Ansible:
- sudo apt-get install software-properties-common
- sudo apt-add-repository ppa:ansible/ansible
- sudo apt-get update
- sudo apt-get install ansible
- ssh-keygen -t rsa ( just press enter for all the questions) ( it will create an ssh private key is id_rsa and a public key in id_rsa.pub) 
- cat .ssh/id_rsa.pub >> .ssh/authorized_keys ( to copy the public keys into the authorized_keys file)


# Install python and boto
- sudo apt-get update
- sudo apt-get upgrade
- sudo apt-get install python3-pip
- sudo apt-get install python-setuptools
- sudo apt-get install python-pip
- sudo pip install virtualenv
Then create the virtual env: 
- virtualenv ansible_vEnv
Activate the virtual env: 
- source ansible_vEnv/bin/activate
- pip3 install boto
- pip install boto
- pip install boto3

# Create the ec2 instances using ansible playbook
- Create User and generate access key
- Create a user - give them full adminstrartor access and then generate and download the secret key
- Install aws cli and configure with secret access key 
- sudo apt  install awscli
- aws configure 
- AWS Access Key ID [None]: 
- AWS Secret Access Key [None]: 
- Default region name [None]: us-east-1
- Default output format [None]:( Here give the secret access key from the csv file that was downloaded,next the access id, next us-east-1e, then enter for next selection)
- now you will be able to see if
cd ~/.aws
ls -ltr
- cat credentials
- cat config
there will be two files credentials and config which contains the key and region
- Connect to the ec2 instance
- aws ec2 describe-instances
- you should be able to see the json output
- Go to the path where ansible is 
- cd /etc/ansible/
- ls - ltr
# Play the ansible playbook
ansible-playbook ec2instanceansible.yml


# Install Docker
- sudo apt-get update
- sudo apt install docker.io
- docker --version
# Install  kubernetes
- sudo apt-get update
- sudo apt-get install -y apt-transport-https ca-certificates curl
- sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
- echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
- sudo apt-get update
- sudo apt-get install -y kubelet kubeadm kubectl
- sudo apt-mark hold kubelet kubeadm kubectl

# Create an ingress that will allow traffic from front end to ETCD database

# Create ClusterRole and ClusterRoleBinding


# Deploy an application and do horizontal scaling to deploy new nodes when the cpu reaches 50%
- autoscaleapp.yaml
 ( this creates an application-cpu deployment and a service and allocates and limits a certain amount of CPU and memory)
- trafficgenerator.yaml ( this creates traffic to increase the cpu used so we can see if new nodes are being deployed)
- kubectl apply -f  autoscaleapp.yaml
- kubectl apply -f  trafficgenerator.yaml
- this will create the pods
- kubectl autoscale deploy application-cpu --cpu-percent=50 --min=1 --max=10 (this will autoscale the application pods)
- kubectl get hpa-application -owide ( this will show the number of replicas, cpu target percent and how much has been used, max/min number of pods)
- kubetcl exec -it traffic-generator sh (to enter this pod)
- apk add --no-cache wrk (to create the traffic)
- kubectl -n kube-system get pods( this will show if a metric server is present)
- kubectl top nodes(this will show how much CPU has been used)
- kubectl get all (will show all the services,deployments, pods,replicasets etc)
- keep increasing the traffic and see if new nodes are being deployed ( kubectl get hpa-application -owide)
 

# Create an ETCD snapshot
- This creates a snapshot backup for the date 
sudo ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key snapshot save /tmp/snapshot-may6th2021.db
Snapshot saved at /tmp/snapshot-may6th2021.db

- This creates a snapshot restore for the backup
sudo ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 --cacert=/etc/kubernetes/pki/etcd/ca.crt  --name=master  --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key --data-dir /var/lib/etcd-from-backup --initial-cluster=master=https://127.0.0.1:2380  --initial-cluster-token=etcd-cluster-1 --initial-advertise-peer-urls=https://127.0.0.1:2380  snapshot restore /tmp/snapshot-may6th2021.db
you will see an ouput like
2021-05-12 17:42:07.631103 I | etcdserver/membership: added member e92d66acd89ecf29 [https://127.0.0.1:2380] to cluster 7581d6eb2d25405b

-Open this config file
sudo vi /etc/kubernetes/manifests/etcd.yaml
edit this file
1. change --data-dir /var/lib/etcd = --data-dir /var/lib/etcd-from-backup
2. add --initial-cluster-token=etcd-cluster-1 (below the 2 lines initial-peer and initial-cluster)
3. change volumeMounts:
    - mountPath: /var/lib/etcd to /var/lib/etcd-from-backup
    - hostPath:
      path: /var/lib/etcd to /var/lib/etcd-from-backup
save and quit :wq       
This will create a new ETCD node in a few seconds

- Grep etcd to see both the old etcd node and the new etcd node
-  ps -ef | grep etcd
This will backup and restore the file and you can store the snapshot on github or any private repo
