# Deploy-your-code-on-a-Docker-Container-using-Jenkins-on-AWS
![68747470733a2f2f696d6775722e636f6d2f486b32386666452e706e67](https://github.com/user-attachments/assets/5aa86fa9-cc41-46bb-97c7-1caa9efdf71e)
In this blog, we are going to deploy a Java Web app on a Docker Container built on an EC2 Instance through the use of Jenkins.

### Agenda
- Setup Jenkins
- Setup & Configure Maven and Git
- Integrating GitHub and Maven with Jenkins
- Setup Docker Host
- Integrate Docker with Jenkins
- Automate the Build and Deploy process using Jenkins
- Test the deployment

### Prerequisites
- AWS Account
- Git/ Github Account with the Source Code
- A local machine with CLI Access
- Familiarity with Docker and Git

### Step 1: Setup Jenkins Server on AWS EC2 Instance

- Setup a Linux EC2 Instance
- Install Java
- Install Jenkins
- Start Jenkins
- Access Web UI on port 8080

Log in to the Amazon management console, open EC2 Dashboard, click on the Launch Instance drop-down list, and click on Launch Instance as shown below:
![image](https://github.com/user-attachments/assets/347aef4c-9c64-4eef-8852-07a81e244615)
Once the Launch an instance window opens, provide the name of your EC2 Instance:
![image](https://github.com/user-attachments/assets/c51475c6-6f63-426a-9179-33f628c4dd22)
Choose an Instance Type. Here you can select the type of machine, number of vCPUs, and memory that you want to have. Select t2.micro which is free-tier eligible.
![image](https://github.com/user-attachments/assets/42c6cdf1-acd2-4008-b5da-c66ec9dfcf9e)
For this demo, we will select an already existing key pair. You can create new key pair if you don’t have:
![image](https://github.com/user-attachments/assets/0d2bafa9-d2e8-4a5b-bb32-7f59ddb180c0)
Now under Network Settings, Choose the default VPC with Auto-assign public IP in enable mode. Create a new Security Group, provide a name for your security group, allow ssh traffic, and custom default TCP port of 8080 which is used by Jenkins.
![image](https://github.com/user-attachments/assets/45db798f-32a9-4260-8bf7-f092771f62ba)
Rest of the settings we will keep them at default and go ahead and click on Launch Instance
![image](https://github.com/user-attachments/assets/2850563f-cbb4-4ab0-892d-68f0e039d27c)
On the next screen you can see a success message after the successful creation of the EC2 instance, click on Connect to instance button:
![image](https://github.com/user-attachments/assets/7be36771-d0af-44f9-a4b4-a4b5c9ad40c6)
Now connect to instance wizard will open, go to SSH client tab and copy the provided chmod and SSH command:
![image](https://github.com/user-attachments/assets/2a14fe67-fdcf-46d0-b616-ed4cad0c2176)
Open any SSH Client in your local machine, take the public IP of your EC2 Instance, and add the pem key and you will be able to access your EC2 machine in my case I am using MobaXterm on Windows:
![image](https://github.com/user-attachments/assets/b3d5712a-a6e3-4a06-be0b-968046731e6f)
After logging in to our EC2 machine we will install Jenkins following the instructions from the official Jenkins website: https://pkg.jenkins.io/redhat-stable/

To use this repository, run the following command

sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo

sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key

### Output
![image](https://github.com/user-attachments/assets/f9abbf4a-d66b-4522-8b10-35c23d207a09)

Now let’s install epel packages for Amazon Linux AMI:
![image](https://github.com/user-attachments/assets/6a947511-30fb-4bc5-9329-375071a87557)

After installing epel packages, let’s install java-openjdk11:
![image](https://github.com/user-attachments/assets/8aaa4aaf-be41-4835-863e-37bde858c5eb)

Let’s check the version of Java now:
![image](https://github.com/user-attachments/assets/b7f62e7b-35fa-44e9-a2f8-a6ba7b837bfd)

Now let’s install Jenkins with the below command as shown in the output:
![image](https://github.com/user-attachments/assets/cf48ba92-41ab-442b-a64c-549808e85cfc)

After successful installation Let’s enable and start Jenkins service in our EC2 Instance:
![image](https://github.com/user-attachments/assets/5ba9a013-e938-45ce-9008-43505217eab8)

Now let’s try to access the Jenkins server through our browser. For that take the public IP of your EC2 instance and paste it into your favorite browser and should see something like this:
![image](https://github.com/user-attachments/assets/2949fb53-3249-4683-ba04-7501f5290dad)

To unlock Jenkins we need to go to the path /var/lib/jenkins/secrets/initialAdminPassword and fetch the admin password to proceed further:
![image](https://github.com/user-attachments/assets/8319a59f-a5c4-4491-a511-a80a5fadedcc)

Now on the Customize Jenkins page, we can go ahead and install the suggested plugins:
![image](https://github.com/user-attachments/assets/dede4113-dd28-4bb7-a956-a19d74f8e6cc)

Now we can create our first Admin user, provide all the required data and proceed to save and continue.
![image](https://github.com/user-attachments/assets/ccf1283e-976d-413e-a504-e0160843afd0)

Now we are ready to use our Jenkins Server.
![image](https://github.com/user-attachments/assets/7a3373e0-8aa9-40a3-9396-ab0f68afaf8a)

