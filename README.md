
## Nginx Proxy Manager 

Install Nginx Proxy Manager Docker, in Open Media Vault 

## Introduction

We’re going to use the setup from [Nginx Proxy Manager](https://nginxproxymanager.com/),
we need to get logged in to Server Machine via SSH. 

## Note: 
This tutorial assumes that you already have Docker and Portainer installed, Over [OpenMediaVault](https://www.openmediavault.org/) 


## Installation

Once you’re logged in via SSH, create a folder called nginx and a new file called docker-compose.yml

```bash
    version: "3"
        services:
        app:
            image: jc21/nginx-proxy-manager:latest
            container_name: nginx-proxy-manager
            volumes:
            - ./config.json:/app/config/production.json
            - ./data:/data
            - ./letsencrypt:/etc/letsencrypt
            depends_on:
            - dbnpm
            ports:
            - 80:80
            - 443:443
            - 81:81
            restart: unless-stopped
        dbnpm:
            image: jc21/mariadb-aria:10.4
            restart: unless-stopped
            environment:
            MYSQL_ROOT_PASSWORD: "YOURPASSWORD"
            MYSQL_DATABASE: "NPMDB"
            MYSQL_USER: "YOURUSERNAME"
            MYSQL_PASSWORD: "YOURSQLPASSWORD"
            volumes:    
            - npm/AppData/Config/NPMDB:/var/lib/mysql #Change 'npm' to your external drive volume if available
            expose:
            - "3306"

```
- Copy the contents of the code block from above into your docker-compose.yml file and make any changes you need for your volume locations.

- Save and close the file.

- Then, to deploy the container, you enter the following in terminal:

```bash
    sudo docker-compose up -d
```

Normally its not required to run this as sudo, but there have been incidents where an error comes up about Docker being installed in a non-standard location and running this as sudo seems to get around that.

Once everything has deployed in the terminal screen, you can jump over to Portainer and take a look at the logs for the nginx_app (or similarly named) and make sure that everything has completed.

If the last line of the logs is this:
```bash

    
    Creating a new JWT key pair...



```

Then you need to wait a bit longer for things to finish up.

Once everything has finished, you can go to http://your-server-ip-address:81

## Default login credentials:
```bash 
        Username: admin@example.com
        Password: changeme
```

Once you’re logged in, you’ll be prompted to change the username and password

That’s it! Now you’ve got NGINX Proxy Manager installed !
