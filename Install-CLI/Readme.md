## Step-1: Install AWS CLI

Link one:  https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html

Link two:  https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html

## Step-2: Configure AWS Command Line using Security Credentials

     -	Go to AWS Management Console --> Services --> IAM
     -	Select the IAM User: kalyan
     -	Important Note: Use only IAM user to generate Security Credentials. Never use "root" user. (Highly not recommended)
     -	Click on Security credentials tab
     -	Click on Create access key
     -	Copy Access ID and Secret access key
     -	Go to command line and provide the required details:
     
 	      # aws configure
	       AWS Access Key ID [None]: ABCDEFGHIAZBERTUCNGG (Replace your creds when prompted)
	       AWS Secret Access Key [None]: uMe7fumK1IdDB094q2sGFhM5Bqt3HQRw3IHZzBDTm (Replace your creds when prompted)
	       Default region name [None]: us-east-1
	       Default output format [None]: json
	
     -	Test if AWS CLI is working after configuring the above     
	      # aws ec2 describe-vpcs
	    
## Step-3: Install kubectl CLI

     -	Kubectl binaries for EKS please prefer to use from Amazon (Amazon EKS-vended kubectl binary)
     -	Reference: https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html
     -	Linux installation:  
       •  download the Amazon EKS vended kubectl binary for your cluster's Kubernetes version from Amazon S3:
              # mkdir kubectlbinar
	          # cd kubectlbinary
	          # curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.21.2/2021-07-05/bin/linux/amd64/kubectl
	     
       •  apply execute permissions:
	          # chmod +x ./kubectl
	      
       •  set the Path by copying to user Home Directory:
	          # mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$PATH:$HOME/bin
	          # echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc
	      
       •  verify the kubectl version:
	          # kubectl version --short –client
	     
## Step-4: Install eksctl CLI
     •	For windows and linux OS, you can refer documentation link:    
          https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html#installing-eksctl
	  
