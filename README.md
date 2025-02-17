#  Deploy-your-code-on-a-Docker-Container-using-Jenkins-on-AWS
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
![image](https://github.com/user-attachments/assets/347aef4c-9c64-4eef-8852-07a81e244615).
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

### Step 2: Integrate GitHub with Jenkins
- Install Git on Jenkins Instance
- Install Github Plugin on Jenkins GUI
- Configure Git on Jenkins GUI
Let’s first install Git on our EC2 instance with the below command:
![image](https://github.com/user-attachments/assets/6375a11c-69ba-4afc-ac67-7e6ffa3a93e6)

We can check the version as shown in the below screenshot:
![image](https://github.com/user-attachments/assets/7e34577b-f3db-455d-b20b-ad5c57ae4508)

To install the GitHub plugin lets go to our Jenkins Dashboard and click on manage Jenkins as shown:
![image](https://github.com/user-attachments/assets/ff97be6f-0ddd-47f3-bfc4-1f2a8e28415a)

On the next page, click on manage plugins:
![image](https://github.com/user-attachments/assets/2f1acee5-cec1-410c-8f95-70780dffc2e4)

Now in order to install any plugin we need to select Available Plugins, search for Github Integration, select the plugin, and finally click on Install without restart as shown below:
![image](https://github.com/user-attachments/assets/14818646-da27-46be-b754-05dc5a74a6a0)

Now let’s configure Git on Jenkins. Go to Manage Jenkins, and click on Global Tool Configuration.
![image](https://github.com/user-attachments/assets/65a8186d-c7ff-4283-99e5-5e8ce0d4a5fe)

Under Git installations, provide the name Git, and under Path, we can either provide the complete path where our Git is installed on the Jenkins machine or just put any name, in my case I put Git to allow Jenkins to automatically search for Git. Then click on Save to complete the installation.
![image](https://github.com/user-attachments/assets/7bec2e84-2125-4e21-a244-d5d92742bfaa)

### Step 3: Integrate Maven with Jenkins
- Setup Maven on Jenkins Server
- Setup Environment Variables JAVA_HOME,M2,M2_HOME
- Install Maven Plugin
- Configure Maven and Java

To install Maven on our Jenkins Server we will switch to the /opt directory and download the Maven package:
![image](https://github.com/user-attachments/assets/5228e747-36d0-4df0-8c18-5db8d5e7e6d1)

Now we will extract the tar.gz file:
![image](https://github.com/user-attachments/assets/86499ffd-6c3e-4b3a-8423-a58d26ec9024)

Now we will set up Environment Variables for our root user in bash_profile in order to access Maven from any location in our Server Go to the home directory of your Jenkins server and edit the bash_profile file as shown in the below steps:
![image](https://github.com/user-attachments/assets/04b9075d-bec7-4408-a23c-c814098ac04b)

In the .bash_profile file, we need to add Maven and Java paths and load these values.
![image](https://github.com/user-attachments/assets/533e5708-88b4-4969-889f-fb69fb4b4755)

To verify follow the below steps:
![image](https://github.com/user-attachments/assets/fe59f056-9b81-41c7-96e7-bdf406f2a844)

With this setup, we can execute maven commands from anywhere on the server:
![image](https://github.com/user-attachments/assets/d67234db-04fd-4fc5-a50b-81662331fac6)

Now we need to update the paths where Java and Maven have been installed in the Jenkins UI. We will first install the Maven Integration Plugin as shown below:
![image](https://github.com/user-attachments/assets/b9f1fb69-4a55-4ac2-b793-d98d557e7c58)

After clicking on Install without restart, go again to manage Jenkins and select Global Tool configuration to set the paths for Java and Maven.
### For JAVA:
![image](https://github.com/user-attachments/assets/fbbf96c3-137e-4bc3-b4ba-132e8a795e29)

### For MAVEN:
![image](https://github.com/user-attachments/assets/f3ec6078-de1f-4294-92b0-e1af9eecb25e)
Click on save and hence we have successfully Integrated Java and Maven with Jenkins.

# Step 4: Setup a Docker Host
- Setup a Linux EC2 Instance
- Install Docker
- Start Docker Services
- Run Basic Docker Commands
Let's first launch an EC2 Instance. We will skip the steps here as we have already shown earlier how to create an EC2 Instance.

Below is the screenshot of our newly created EC2 Instance on which we will install Docker:
![image](https://github.com/user-attachments/assets/32e6fc81-8aef-4767-ae27-11c70c61b187)

We will first install Docker on this EC2 Instance:
![image](https://github.com/user-attachments/assets/66e3d7c8-4963-4a99-976a-f5e1a59ec0ee)

After the successful installation of Docker, let’s verify the version of Docker:
![image](https://github.com/user-attachments/assets/76b62205-930f-4cab-b509-8b28582e3383)

Also, let’s enable and start the Docker Service:
![image](https://github.com/user-attachments/assets/fef7b77c-8680-4dad-8ec1-28a10d9e301d)

### Create Tomcat Docker Container
In my previous blog, I deployed a Java code on a Tomcat VM, here in this blog we will deploy on a Tomcat Docker container.

We will first pull the official Tomcat docker image from the Docker Hub and then run the container out of the same image.
![image](https://github.com/user-attachments/assets/18e53f86-dc0a-41b8-abb5-0ea94d500943)

Let’s now create a Container from the same Image with the command:
docker run -d --name tomcat-container -p 8081:8080 tomcat
![image](https://github.com/user-attachments/assets/28c79c47-2c08-4c6a-99ae-c1287f3e5d48)

The above command runs a docker container in detached mode with the name tomcat-container and we are exposing port 8081 of our host machine with port 8080 of our container and it's using the latest image of tomcat.

Let's verify the running container on our EC2 machine:
![image](https://github.com/user-attachments/assets/77ed70e0-4af5-42f3-95ff-555d8d6f0f58)

Before accessing our container from the browser we need to allow port 8081 in the Security Group of our EC2 docker-host machine.

Go to the Security group of your EC2 machine and click on Edit inbound rules:
![image](https://github.com/user-attachments/assets/5276d97d-de37-47e2-afba-30d724ee1d1f)

Click on Add rule, select Custom TCP as type, and in port range put 8081–9000 in case we need it in the future, and under source select from anywhere and then click on Save rules to proceed:
![image](https://github.com/user-attachments/assets/71a4bae8-0aec-4ce4-8e74-4b3e5d5121d7)

Now let’s take the public IP of our Docker-host EC2 machine and with port 8081 access it from our browser:
![image](https://github.com/user-attachments/assets/81a80e0d-0157-4804-b315-40d5feb5e5d2)

From the above screenshot, you can see that although there is a 404 error, it also displays Apache Tomcat at the bottom which means the installation is successful however this is a known issue with the Tomcat docker image that we will fix in the next steps.

The above issue occurs because whenever we try to access the Tomcat server from the browser it will look for the files in /webapps directory which is empty and the actual files are being stored in /webapps.dist.

So in order to fix this issue we will copy all the content from webapps.dist to webapps directory and that will resolve the issue.

Let's access our tomcat container and perform the steps as shown below:
![image](https://github.com/user-attachments/assets/d7ee4fbd-3c83-46ef-973e-c3046dc1dad5)

Once we are in the tomcat container go to the /webapps.dist directory and copy all the content to webapps directory:
![image](https://github.com/user-attachments/assets/16a7f2ce-bc4f-4715-9873-a45e18ada426)

After that we should be able to access our tomcat docker container:
![image](https://github.com/user-attachments/assets/95f051e1-216b-4d56-94af-4d248d3f8dec)

Somehow if we stop this container and start another container with the same Image on a different port we will face the same issue of 404 error. This happens because every time we launch a new container we are using a new Image as the previous container gets deleted.

This issue can be solved by creating our own Docker Image with the appropriate changes required to run our container. This can be achieved by creating a Dockerfile on which we can mention the steps required to build the docker image and from that Image, we can run the container.

### Create a Customized Dockerfile for Tomcat:

To create the Dockerfile we will use the official Image of Tomcat and with it will mention the step to copy the contents from the directory /webapps.dist to /webapps:
FROM  tomcat:latest
RUN cp -R /usr/local/tomcat/webapps.dist/* /usr/local/tomcat/webapps
![image](https://github.com/user-attachments/assets/d8d18b05-eb8e-4831-b3f2-2b68d5dd8a70)

Now let's build the Docker Image using this Dockerfile using the below command:
docker build -t tomcatserver .
![image](https://github.com/user-attachments/assets/165728bf-feb0-4af7-8267-1238336a56dd)

Let’s verify with the docker images command:
![image](https://github.com/user-attachments/assets/db368d71-41cc-4886-928a-c3cbeeb9821d)

Now is the time to run the docker container out of our customized docker image which we built using our dockerfile. The command is as:
docker run -d --name tomcat-server -p 8085:8080 tomcatserver

Here you should remember that we have already allowed port range 8081–9000 in the security group of our docker EC2 Instance.
![image](https://github.com/user-attachments/assets/63d3795f-898b-4846-8960-afb9ad042f40)

Let’s verify using the docker ps command:
![image](https://github.com/user-attachments/assets/19415d5c-a0b5-4857-95a1-f285444a90a6)

Also, let's try to access the Tomcat server on port 8085 from the browser:
![image](https://github.com/user-attachments/assets/e00aba26-8219-4cc2-ad2f-6e477283a45f)
Hence we should be now able to launch as many times the same container without facing any issues by utilizing our customizable Dockerfile.

### Step 5: Integrate Docker with Jenkins
- Create a dockeradmin user
- Install the “Publish Over SSH” plugin
- Add Dockerhost to Jenkins “configure systems

Let's first create a dockeradmin user and create a password for it as well.
![image](https://github.com/user-attachments/assets/0c72df43-94c8-450c-a833-993bdb033d8e)
Now let’s add this user to the Docker group with the below command:

usermod -aG docker dockeradmin

Now in order to access the server using the newly created User we need to allow password-based authentication. For that, we need to do some changes in the /etc/ssh/sshd_config file.
![image](https://github.com/user-attachments/assets/a5b62200-e68c-4d44-aa7c-96f4db8f8d58)

As you can see above we need to uncomment PasswordAuthentication yes and comment out PasswordAuthentication no.

Once we have updated the config file we need to restart the services with the command:
service sshd reload
Redirecting to /bin/systemctl reload sshd.service

Now we would be able to log in using the dockeradmin user credentials:
![image](https://github.com/user-attachments/assets/77e173b5-16da-49e6-808f-b8bf1d9c6117)

Now next step is to integrate Docker with Jenkins, for that we need to install the “Publish Over SSH” plugin:

Go to Manage Jenkins > Manage Plugins > Available plugins and search for publish over ssh plugin:
![image](https://github.com/user-attachments/assets/a4cf713f-6af8-401a-b6bb-6ea40fb3322e)

Click on Install without restart to install the plugin.

Now we need to configure our dockerhost in Jenkins. For that go to Manage Jenkins > Configure System:
![image](https://github.com/user-attachments/assets/324e0d70-bbf1-4019-a118-62c22a7fe4d5)

On the next page after scrolling down you would be able to see Publish over SSH section where you need to add a new SSH server with the info as shown below:
![image](https://github.com/user-attachments/assets/dc58f170-9cf7-4fd2-a7ec-f5056cf6a039)

It should be noted that it's best practice to use ssh keys however for this demo we are using password-based authentication. In the above screenshot, we have provided details of our Docker host which we created on EC2 Instance. Also, note the use of private IP under Hostname as both our Jenkins Server and Dockerhost are on the same subnet. We can also use Public IP here.

Click on Apply and Save to proceed. With this, our Docker integration with Jenkins is successfully accomplished.




