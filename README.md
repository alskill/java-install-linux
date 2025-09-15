# java-install-yum

# install amazon linux 2023


Installing Jenkins on an Amazon Linux 2023 EC2 instance is a straightforward process using MobaXterm to SSH into your instance. Since Amazon Linux 2023 uses DNF (which yum is an alias for) and provides Amazon Corretto 11 directly, you'll install both Jenkins and its Java dependency using the same package manager.

Step-by-Step Installation
Connect to Your EC2 Instance
Use MobaXterm to connect to your EC2 instance via SSH. Make sure you have your private key (.pem file) configured in your MobaXterm session.

Update the System
It's a best practice to update your system packages before installing new software.   

Bash

sudo dnf update -y
Install Java
Jenkins requires a Java Development Kit (JDK) to run. Amazon Corretto 11 is the recommended and readily available JDK for Amazon Linux 2023.   

Bash

sudo yum install java-11-amazon-corretto-devel -y
The -devel package includes the JDK, which is necessary for Jenkins.

Add the Jenkins Repository
Jenkins is not in the default Amazon Linux repositories, so you need to add its official repository.   

Bash

sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
Import the Jenkins GPG Key
To verify the integrity of the downloaded packages, import the Jenkins GPG key.   

Bash

sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
Install Jenkins
Now you can install Jenkins using the yum command.   

Bash

sudo yum install jenkins -y
Start and Enable the Jenkins Service
After installation, start the Jenkins service and enable it to run automatically on system boot.   

Bash

sudo systemctl start jenkins
sudo systemctl enable jenkins
sudo systemctl status jenkins
The last command, sudo systemctl status jenkins, will confirm if the service is active and running.

Configure the Security Group 🛡️
Jenkins runs on port 8080 by default. To access the web interface from your browser, you must add an inbound rule to your EC2 instance's security group to allow traffic on port 8080.   

Navigate to your EC2 instance in the AWS Management Console.

Click on the Security tab.

Click on the security group link.

Select the Inbound rules tab and click Edit inbound rules.

Add a new rule with:

Type: Custom TCP

Port range: 8080   

Source: You can select "My IP" for a more secure connection or "Anywhere IPv4" if you're comfortable with it being publicly accessible.

Click Save rules.

Access Jenkins 🚀
Open a web browser and navigate to http://<your-ec2-public-ip>:8080. You should see the Jenkins setup wizard. To unlock it, you'll need the initial administrator password.   

Retrieve the Initial Admin Password
On your MobaXterm terminal, run the following command to get the password from the Jenkins log file.

Bash

sudo cat /var/lib/jenkins/secrets/initialAdminPassword
