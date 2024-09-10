# kops-install-aws
kops installation and deletion process
# what-is-kops
https://kops.sigs.k8s.io/#what-is-kops

kops will not only help you create, destroy, upgrade and maintain production-grade, highly available, Kubernetes cluster, but it will also provision the necessary cloud infrastructure.
AWS (Amazon Web Services) and GCE (Google Cloud Platform) are currently officially supported, with DigitalOcean, Hetzner and OpenStack in beta support, and Azure in alpha.

# How to Install kOps - high level steps
- Create E2 instance
- Create S3 bucket
- Create key pair - (EC2 > Key pairs > Create key pair)
- Created hosted zone (Route 53 > Hosted zones) & updare NS to resolve subdomain- (example kubevpro.learndevops.shop)

  reference doc - https://kops.sigs.k8s.io/getting_started/aws/#scenario-2-setting-up-route53-for-a-domain-purchased-with-another-registrar
  
- Loging to EC2 instance and generate ssh key with ssh-keygen which will be used by kOps for cluster login
- install aws cli - https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
  
  curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
  
  unzip awscliv2.zip
  
  sudo ./aws/install
  
- Create IAM user with necessary permissions - https://kops.sigs.k8s.io/getting_started/aws/#setup-iam-user
- run "aws configure" to update "aws_access_key_id" and "aws_secret_access_key"
- install kubectl - https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html
- install kops - https://kops.sigs.k8s.io/getting_started/install/
- Create cluster - The below command will generate a cluster configuration, but will not start building it.
  
  Example
  
  kops create cluster --name=kubevpro.learndevops.shop --state=s3://s3-bucket-name --zones=ap-south-1a,ap-south-1b --node-count=2 --node-size=t3.small --control-plane-size=t3.medium --dns-zone=kubevpro.learndevops.shop --node-volume-size=8 --control-plane-volume-size=8
  
- run following command to build the cluster
  
  kops update cluster --name=kubevpro.learndevops.shop --state=s3://s3-bucket-name --yes --admin
  
- validate cluster
  
  kops validate cluster --state=s3://s3-bucket-name --wait 10m
  
- The configuration for your cluster was automatically generated and written to ~/.kube/config
- check the nodes
  
  kubectl get nodes
  
- command to delete cluster
  
  kops delete cluster --name=kubevpro.learndevops.shop --state=s3://s3-bucket-name --yes
  
- shutdown EC2 instance
- Delete hosted zone under route 53 if not in use
