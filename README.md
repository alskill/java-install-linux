# Jenkins + Git + Maven Setup (Amazon Linux)

---

# Step 1: Change Hostname

Check current hostname.

```bash
cat /etc/hostname
```

Edit hostname.

```bash
sudo vi /etc/hostname
```

Example:

```
jenkins_server
```

Set hostname immediately.

```bash
sudo hostnamectl set-hostname jenkins_server
```

Verify.

```bash
hostname
```

---

# Step 2: Install Java

Update packages.

```bash
sudo yum update -y
```

Install Java 17.

```bash
sudo yum install java-17-amazon-corretto-devel -y
```

Verify Java.

```bash
java -version
```

---

# Step 3: Install Git

Install Git.

```bash
sudo yum install git -y
```

Verify.

```bash
git --version
```

Find Git path.

```bash
which git
```

Example output:

```
/usr/bin/git
```

---

# Step 4: Install Git Plugin in Jenkins

Manage Jenkins

→ Plugins

→ Available Plugins

→ Search **Git**

→ Install

---

# Step 5: Configure Git in Jenkins

Manage Jenkins

→ Tools

→ Git Installations

Name

```
Git
```

Path to Git

```
/usr/bin/git
```

Click **Apply** → **Save**

---

# Step 6: Pull Code from GitHub

Create

```
New Item
```

Select

```
Freestyle Project
```

Source Code Management

```
Git
```

Repository URL

```
https://github.com/username/repository.git
```

Credentials

- Leave blank for Public Repository
- Add GitHub credentials for Private Repository

Click

```
Apply
Save
```

---

# Step 7: Build Job

Click

```
Build Now
```

Open

```
Console Output
```

---

# Step 8: Verify Repository Cloned

```bash
cd /var/lib/jenkins/workspace
```

```bash
ls -l
```

```bash
cd <project-name>
```

Example

```bash
cd webapp
```

```bash
ls -l
```

---

# Step 9: Install Apache Maven

Go to /opt

```bash
cd /opt
```

Download Maven.

```bash
sudo wget https://downloads.apache.org/maven/maven-3/3.9.11/binaries/apache-maven-3.9.11-bin.tar.gz
```

Extract.

```bash
sudo tar -xvzf apache-maven-3.9.11-bin.tar.gz
```

Rename folder.

```bash
sudo mv apache-maven-3.9.11 maven
```

Verify.

```bash
ls -l
```

Go inside Maven.

```bash
cd /opt/maven
```

```bash
ls -l
```

Verify Maven.

```bash
./bin/mvn -v
```

---

# Step 10: Configure Maven Environment Variables

Go to home directory.

```bash
cd ~
```

Open profile.

```bash
vi ~/.bash_profile
```

Add the following.

```bash
export M2_HOME=/opt/maven
export M2=/opt/maven/bin
export JAVA_HOME=/usr/lib/jvm/java-17-amazon-corretto
export PATH=$PATH:$JAVA_HOME/bin:$M2_HOME/bin
```

Verify Java location if needed.

```bash
find /usr/lib/jvm -name java
```

or

```bash
readlink -f $(which java)
```

Reload profile.

```bash
source ~/.bash_profile
```

Verify.

```bash
echo $PATH
```

```bash
mvn -v
```

---

# Step 11: Install Maven Plugin

Manage Jenkins

→ Plugins

→ Available Plugins

Search

```
Maven Integration
```

Install

---

# Step 12: Configure Maven in Jenkins

Manage Jenkins

→ Tools

## JDK

Name

```
Java-17
```

JAVA_HOME

```
/usr/lib/jvm/java-17-amazon-corretto
```

## Maven

Name

```
Maven-3.9.11
```

MAVEN_HOME

```
/opt/maven
```

Click

```
Apply
Save
```

---

# Step 13: Create Maven Project

```
New Item
```

Choose

```
Maven Project
```

Description

```
Build Java Maven Project
```

Source Code Management

```
Git
```

Repository URL

```
https://github.com/username/repository.git
```

Goals

```
clean install
```

Click

```
Apply
Save
```

---

# Step 14: Build Project

```
Build Now
```

Open

```
Console Output
```

---

# Step 15: Verify WAR File

```bash
cd /var/lib/jenkins/workspace
```

```bash
cd webapp
```

```bash
cd target
```

```bash
ls -l
```

Example output

```
webapp.war
classes/
generated-sources/
maven-status/
```

The **.war** file is the deployable Java web application created by Maven.
