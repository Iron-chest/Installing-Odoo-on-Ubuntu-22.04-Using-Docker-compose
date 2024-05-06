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
   And paste this commands
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
2. Make the docker.sh script executable
```
sudo chmod +x docker.sh

```
3. Run the docker.sh script
```
./docker.sh 

```
docker --version
sudo vim docker-compose.sh
    
    sudo curl -SL https://github.com/docker/compose/releases/download/v4.27.0/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose
    sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

sudo chmod +x docker-compose.sh
./docker-compose.sh
docker-compose -v
sudo chmod 666 /var/run/docker.sock
mkdir ~/odoo
cd ~/odoo
sudo vim docker-compose.yml
    version: '3'
services:
  odoo:
    image: odoo:15.0
    env_file: .env
    depends_on:
      - postgres
    ports:
      - "127.0.0.1:8069:8069"
    volumes:
      - data:/var/lib/odoo
  postgres:
    image: postgres:13
    env_file: .env
    volumes:
      - db:/var/lib/postgresql/data/pgdata

volumes:
  data:
  db:

vim .env
    # postgresql environment variables
    POSTGRES_DB=postgres
    POSTGRES_PASSWORD=a_strong_password_for_user
    POSTGRES_USER=odoo
    PGDATA=/var/lib/postgresql/data/pgdata

    # odoo environment variables
    HOST=postgres
    USER=odoo
    PASSWORD=a_strong_password_for_user

openssl rand -base64 30
docker-compose up -d

Access Odoo UI from the http://ipaddress:8069
Mail: tolani.akintayo.bincom@gmail.com
password: jxyp-jfpa-75t5

docker-compose stop

