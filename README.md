# Installing Odoo 17 on Ubuntu 22.04 Using Docker-compose
## STEP 1: UPDATE THE SYSTEM
1. Update the packages in the apt repo
```
sudo apt update -y && sudo apt upgrade -y
```
2. Change the hostname of your server
```
sudo vim /etc/hostname
```
3. Reboot your server to adopt new changes
```
sudo reboot
```
## STEP 2: INSTALLING DOCKER 
1. Create a docker.sh file to automate the installation of docker
```sudo vim docker.sh```
2. And paste this commands
```
    #Add Docker's official GPG key:
    sudo apt-get update -y
    sudo apt-get install ca-certificates curl
    sudo install -m 0755 -d /etc/apt/keyrings -y
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
    sudo chmod a+r /etc/apt/keyrings/docker.asc

    #Add the repository to Apt sources:
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
    $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update -y
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
    sudo docker run hello-world
```
3. Make the docker.sh script executable
```
sudo chmod +x docker.sh
```
4. Run the docker.sh script
```
./docker.sh 
```
5. Check docker version
```
docker --version
```
# STEP 3: INSTALLING DOCKER-COMPOSE
1. Create a docker-compose.sh file to automate the installation of docker-compose
`sudo vim docker-compose.sh`
2. And paste this commands
    
    sudo curl -SL https://github.com/docker/compose/releases/download/v4.27.0/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose
    sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
3. Make the docker-compose.sh script executable
```
sudo chmod +x docker-compose.sh
```
4. Run the docker-compose script
```
./docker-compose.sh
```
5. Check the version of docker-compose
```
docker-compose -v
```
6. Run this command to allow sudo privileges for the current user (ubuntu)
``` sudo chmod 666 /var/run/docker.sock
```
# STEP 4: INSTALLING ODOO AND POSTGRES USING DOCKER-COMPOSE
1. Create a directory named odoo and change dir to odoo
```
mkdir ~/odoo
cd ~/odoo
```
2. Create a docker-compose .yml file to automate the provisioning of Odoo and postgres docker container.
3. Create a docker-compose file named docker-compose.yml
`sudo vim docker-compose.yml`
4. Paste the following details.
```
version: '3.7'
services:
  odoo:
    image: odoo:17.0
    env_file: .env
    depends_on:
      - postgres
    ports:
      - "8069:8069"
    volumes:
      - odoo-web-data:/var/lib/odoo
  postgres:
    image: postgres:15
    env_file: .env
    volumes:
      - db:/var/lib/postgresql/data/pgdata

volumes:
  odoo-web-data:
  db:
```
5. Create environmenatl variables to avoid reveal sensitive details
6. Create a this `sudo vim .env`
7. Paste this details
```
    #postgresql environment variables
    POSTGRES_DB=postgres
    POSTGRES_PASSWORD=a_strong_password_for_user
    POSTGRES_USER=odoo
    PGDATA=/var/lib/postgresql/data/pgdata

    #odoo environment variables
    HOST=postgres
    USER=odoo
    PASSWORD=a_strong_password_for_user
```
8. Use this commands to generate a strong password that can be used in the .env file
```
openssl rand -base64 30
```
9. Start the Odoo and postgres containers with the docker-compose command
``` 
docker-compose up -d

```
# STEP 5: ACCESS ODOO ON YOUR BROWSER
1. Access Odoo UI from the http://ipaddress:8069

<!-- Mail: tolani.akintayo.bincom@gmail.com
password: jxyp-jfpa-75t5 -->

2. To stop the Odoo and postgres containers with docker-compose command
``` docker-compose stop
```

