## Launch EC2 "t2.micro" Instance and In Sg, Open port "5000" for Python Application 
# Backend-Node.js Application server

## Setup your Application Database by executing "initdb.sql" script from Application-server

Step:1 ==> install "MYSQL-Client" for communicate with MYSQL Database
```
sudo yum update -y
sudo wget https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm
sudo dnf install mysql80-community-release-el9-1.noarch.rpm -y
sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2023
sudo dnf install mysql-community-client -y
```
Step:2 ==> Execute your "init.sql" script for your Application DB setup

```
mysql -u root -p<root-Password> < initdb.sql
```

## Install Node and NPM
```
sudo yum update -y
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
. ~/.nvm/nvm.sh
nvm install 16
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
nvm install --lts
nvm use --lts
```
### Check Node Version
```
node -v
npm -v
```
### Install Git
```
sudo yum install git -y
```
## Get the Code
```
git clone https://github.com/techizone-Medium-Project-org/Nodejs-3-tier-UMS-App.git
cd Nodejs-3-tier-UMS-App
sudo chown -R ec2-user:ec2-user /home/ec2-user/Nodejs-3-tier-UMS-App
```

## Add .env for DB Credentials 
```
cd api
```
```
sudo vim .env
```
```
DB_HOST=<DB-Private-IP>
DB_USER=appuser
DB_PASSWORD=Aditya
DB_NAME=crud_app
JWT_SECRET=digistackSuperSecretKey
```

## Install Dependencies
```
npm install
```

## Start the App
```
npm start
```
HERE it is not recommend in Production, so we follow the HA in Production

Start Backend Application
```
npm install -g pm2
```
To run these Backend Application up and Running we use Linux service
```
which pm2
sudo cp -r  ~/.local/bin/pm2 /usr/local/bin/
```

```
sudo vim /etc/systemd/system/backend.service
```
```
[Unit]
Description=pm2 Node.js App
After=network.target

[Service]
User=ec2-user
Group=ec2-user
WorkingDirectory=/home/ec2-user/Nodejs-3-tier-UMS-App/api
ExecStart=/usr/local/bin/pm2 start app.js
Restart=always

[Install]
WantedBy=multi-user.target
```
Enable backend service
```
sudo systemctl daemon-reload
sudo systemctl enable backend
sudo systemctl start backend
sudo systemctl status backend
```
