# java-install-yum

# install amazon linux 2023

✅ Step 1: Update system

sudo dnf update -y

✅ Step 2: Install Java 17 or 21

Jenkins (latest LTS) requires Java 17+. Install Amazon Corretto 17:

sudo dnf install java-17-amazon-corretto -y


Check:

java -version


You should see openjdk version "17...".

✅ Step 3: Add Jenkins repository

Jenkins is not in default Amazon Linux repos, so add the official Jenkins repo:
3 — Add the Jenkins repository and import the signing key

# add repo
sudo curl -fsSL https://pkg.jenkins.io/redhat-stable/jenkins.repo -o /etc/yum.repos.d/jenkins.repo
# import repo GPG key
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key

✅ Step 4: Install Jenkins

sudo dnf install jenkins -y


✅ Step 5: Adjust service file (Java 17 path)

By default, the unit may still point to /usr/bin/java. Verify:

cat /usr/lib/systemd/system/jenkins.service | grep ExecStart

If it doesn’t use Java 17, edit:

sudo vi /usr/lib/systemd/system/jenkins.service


Replace ExecStart with:

ExecStart=/usr/lib/jvm/java-17-amazon-corretto.x86_64/bin/java -Djava.awt.headless=true -jar /usr/share/java/jenkins.war --httpPort=8080


Save with :wq.

✅ Step 6: Reload and start Jenkins

sudo systemctl daemon-reload
sudo systemctl enable jenkins
sudo systemctl start jenkins

✅ Step 7: Check if it’s running
sudo systemctl status jenkins -l


You should see:       
Active: active (running)
Also confirm port 8080 is listening:

# check listening port

ss -ltnp | grep 8080 || sudo ss -ltnp | grep java

# follow logs

sudo journalctl -u jenkins -f

# or older logs

sudo tail -n 200 /var/log/jenkins/jenkins.log


✅ Step 8: Open firewall (if needed)

If using firewalld:

sudo firewall-cmd --permanent --add-port=8080/tcp
sudo firewall-cmd --reload


Or if on AWS EC2 → open port 8080 in the security group.

✅ Step 9: Unlock Jenkins

Get the initial admin password:

sudo cat /var/lib/jenkins/secrets/initialAdminPassword


Paste the password, complete setup wizard, install plugins, and create admin user.


Private IP (inside AWS VPC):   hostname -I
To check Public IP of EC2:   curl -s http://checkip.amazonaws.com

