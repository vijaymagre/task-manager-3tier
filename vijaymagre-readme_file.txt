# Make 3 Tier Simple Application

########################################################################################## 

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* make 3 tier
Simple application \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
\##########################################################################################

## First Step

1.  Open the local project folder (`3-tier-app`) in VS Code (File → Open
    Folder).
2.  Push this project folder (`3-tier-app`) to GitHub Repo.
3.  Make an empty repo using name `task-manager-3tier`.
4.  After creating repo copy repo URL
    (`https://github.com/SanketParbat1211/task-manager-3tier.git`).
5.  Open VS Code Terminal and run following command one by one:

``` bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/SanketParbat1211/task-manager-3tier.git
git push -u origin main
```

6.  When you push your code to GitHub, Git may ask you to log in.

-   Username: Enter your GitHub username. (`SanketParbat1211`)
-   Password: Do not enter your GitHub account password. Instead, use a
    Personal Access Token (PAT).

If you don't have a token:

-   Go to GitHub.
-   Click your profile photo → Settings.
-   Scroll down and click Developer settings.
-   Click Personal access tokens.
-   Select Tokens (classic).
-   Click Generate new token.
-   Select the repo checkbox.
-   Click Generate token.
-   Copy the token immediately and save it somewhere safe.
-   When Git asks for a password during `git push`, paste this token
    instead of your GitHub password.

## Second Step

1.  Lunch EC2 Instance.

2.  Give name `task-manager-server`.

3.  Choose Ubuntu Server 22.04 LTS.

4.  Instance type: `t2.micro` or `t2.medium`.

5.  Network settings --- "Edit inbound rule":

    -   Type: SSH, Port: 22, Source: My IP
    -   Type: Custom TCP, Port: 4200, Source: Anywhere (0.0.0.0/0)
    -   Type: Custom TCP, Port: 5000, Source: Anywhere (0.0.0.0/0)

6.  Launch Instance.

7.  Connect to terminal using following command:

``` bash
ssh -i "put .pem file access" ubuntu@EC2_public_ip
```

My example:

``` bash
ssh -i "C:\Users\LENOVO\OneDrive\aws-keys\awskey.pem" ubuntu@EC2_public_ip
```

## Third Step (check app/website run locally)

### Open 1 terminal

1.  Update the System:

``` bash
sudo apt update
```

2.  Install Node.js and Git:

``` bash
sudo apt install -y nodejs npm git
```

3.  After complete please check versions:

``` bash
node -v
npm -v
git --version
```

4.  Install MongoDB:

``` bash
wget -qO - https://www.mongodb.org/static/pgp/server-7.0.asc | sudo apt-key add -
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
sudo apt update
sudo apt install -y mongodb-org
```

5.  Start MongoDB:

``` bash
sudo systemctl start mongod
sudo systemctl status mongod
```

6.  Install Angular CLI:

``` bash
sudo npm install -g @angular/cli
```

## Fourth Step

1.  Code clone from GitHub Repo:

``` bash
git clone https://github.com/SanketParbat1211/task-manager-3tier.git
```

2.  Check folder and files:

``` bash
cd task-manager-3tier
ls
```

(See frontend backend names.)

## Fifth Step

### Backend setup

``` bash
cd backend
npm install
cp .env.example .env
```

### Run Backend

``` bash
npm run dev
```

Output:

``` text
MongoDB Connected: ...
Server running on port 5000
```

## Sixth Step

### Open 2 another terminal

1.  Connect to terminal using following command:

``` bash
ssh -i "put .pem file access" ubuntu@EC2_public_ip
```

My example:

``` bash
ssh -i "C:\Users\LENOVO\OneDrive\aws-keys\awskey.pem" ubuntu@EC2_public_ip
```

2.  Frontend Setup:

``` bash
cd task-manager-3tier/frontend
npm install
```

3.  Start Frontend:

``` bash
npm start -- --host 0.0.0.0
```

Output:

``` text
✔ Compiled successfully.
```

## Seventh Step

1.  Check website run on browser (Chrome):

``` text
http://<EC2-Public-IP>:4200
```

2.  If backend doesn't connect then do follow:

``` bash
nano ~/task-manager-3tier/frontend/src/environments/environment.ts
```

See code:

``` typescript
export const environment = {
    production: false,
    apiUrl: 'http://localhost:5000/api'
};
```

Changes: Change localhost into your Ubuntu public IP.

3.  Retry again.

########################################################################################## 

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
Docker
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
\##########################################################################################

Every time if you stop your instance then restart the instance the new
public IP is created, so after that you need to change following file
and add public IP there.

### If backend doesn't connect

``` bash
nano ~/task-manager-3tier/frontend/src/environments/environment.ts
```

See code:

``` typescript
export const environment = {
    production: false,
    apiUrl: 'http://localhost:5000/api'
};
```

Changes: Change localhost into your Ubuntu public IP.

After changing file do:

``` text
ctrl S ---> ctrl X
```

## Docker --- 1st Step

1.  Ubuntu update:

``` bash
sudo apt update
sudo apt upgrade -y
```

2.  Docker Package Install:

``` bash
sudo apt install -y ca-certificates curl
```

3.  Create Docker GPG key directory:

``` bash
sudo install -m 0755 -d /etc/apt/keyrings
```

4.  Download Docker GPG key:

``` bash
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

5.  Add Docker repository:

``` bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

6.  Repository update:

``` bash
sudo apt update
```

7.  Docker Install:

``` bash
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

8.  Docker Version:

``` bash
docker --version
```

9.  Docker Compose version:

``` bash
docker compose version
```

10. Docker service start:

``` bash
sudo systemctl start docker
```

11. Docker automatically start after EC2 restart:

``` bash
sudo systemctl enable docker
```

Status check:

``` bash
sudo systemctl status docker
```

`~q` --- exit

12. Docker use without Sudo:

``` bash
sudo usermod -aG docker $USER
newgrp docker
```

13. Check the containers:

``` bash
docker ps
```

14. Docker Test:

``` bash
docker run hello-world
```

Output:

``` text
docker run hello-world
```

## Docker --- 2st Step

1.  If you see the medium tier EC2 instance memory full then check the
    memory:

``` bash
df -h
```

2.  Remove unwanted files:

``` bash
rm -rf backend/node_modules
rm -rf frontend/node_modules
docker system prune -a -f
sudo apt clean
sudo apt autoremove -y
```

3.  After using above commands still getting error then check where
    exactly memory full:

``` bash
du -sh ~/task-manager-3tier/* 2>/dev/null
du -sh /var/lib/docker 2>/dev/null
du -sh /var/lib/containerd 2>/dev/null
sudo du -sh /* 2>/dev/null | sort -rh | head -15
```

-   `du -sh ~/task-manager-3tier/* 2>/dev/null` --- Show the size of
    each item in the project folder.
-   `du -sh /var/lib/docker 2>/dev/null` --- Check how much disk space
    Docker is using.
-   `du -sh /var/lib/containerd 2>/dev/null` --- Check how much disk
    space Docker is using.
-   `sudo du -sh /* 2>/dev/null | sort -rh | head -15` --- This displays
    a list of the largest directories in the entire system.

## Docker --- 3st Step

1.  If storage memory full then Increase EC2 Instance EBS volume size
    8GB to 20 GB.

2.  Once done increase size then follow command in terminal:

``` bash
lsblk
sudo growpart /dev/xvda 1
sudo resize2fs /dev/root
df -h
```

-   `lsblk` --- Show the disk partitions and their sizes.
-   `sudo growpart /dev/xvda 1` --- Resize partition 1 on disk
    `/dev/xvda` to use the available space.
-   `sudo resize2fs /dev/root` --- Resize the filesystem to match the
    expanded partition size.
-   `df -h` --- Verify if the disk resize was done properly.

3.  After successfully added space then rerun Docker file and check the
    output:

``` bash
cd ~/task-manager-3tier
docker compose up --build
```

After you can copy public IP and check on browser using (example):

``` text
http://13.201.100.207:4200/
```

########################################################################################## 

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
Jenkins
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
\##########################################################################################

Every time if you stop your instance then restart the instance the new
public IP is created, so after that you need to change following file
and add public IP there.

### If backend doesn't connect

``` bash
nano ~/task-manager-3tier/frontend/src/environments/environment.ts
```

See code:

``` typescript
export const environment = {
    production: false,
    apiUrl: 'http://localhost:5000/api'
};
```

Changes: Change localhost into your Ubuntu public IP.

## Jenkins --- 1st Step

The original notes included a Jenkins password, username and email. The
password and account details have been omitted from this README for
security. Do not commit Jenkins passwords or initial admin passwords to
a public GitHub repository. Store credentials securely and rotate any
credentials that have already been exposed.

## Jenkins --- 2nd Step

1.  System update:

``` bash
sudo apt update
```

2.  Java install:

``` bash
sudo apt install -y fontconfig openjdk-17-jre
java -version
```

3.  Make folder for Jenkins keyring:

``` bash
sudo mkdir -p /etc/apt/keyrings
```

4.  Download Valid Jenkins signing key:

``` bash
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian/jenkins.io-2026.key
```

5.  Add Jenkins repository:

``` bash
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" https://pkg.jenkins.io/debian binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```

6.  Update Package List Again:

``` bash
sudo apt update
```

7.  Run Jenkins Manually:

``` bash
sudo -u jenkins java -jar /usr/share/java/jenkins.war --httpPort=8080
```

8.  Choose browser, put IP address and then use port Jenkins for
    dashboard.

Example:

``` text
http://<EC2-Public-IP>:8080
```

##################################################################################################################### 

\*\*\*\*\*\*\* if stop instance and after you want to relogin to project
then do following steps
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
\#####################################################################################################################

1.  If relogin then take public IP from EC2 Instance.

2.  Use terminal and login via SSH:

``` bash
ssh -i "C:\Users\LENOVO\OneDrive\aws-keys\awskey.pem" ubuntu@65.2.142.227
```

3.  Then change in `environment.ts` file:

``` bash
nano ~/task-manager-3tier/frontend/src/environments/environment.ts
```

See code:

``` typescript
export const environment = {
    production: false,
    apiUrl: 'http://localhost:5000/api'
};
```

Changes: Change localhost into your Ubuntu public IP.

4.  Then go to project folder and run Docker:

``` bash
cd ~/task-manager-3tier
docker compose up --build -d
docker ps
```

5.  Go to browser and check the app is running or not.

Example:

``` text
http://13.201.100.207:4200/
```

6.  Check Jenkins Service is status start or not:

``` bash
sudo systemctl status Jenkins
```

7.  
