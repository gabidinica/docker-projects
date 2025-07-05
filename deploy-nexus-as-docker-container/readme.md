# Demo Project

## Deploy Nexus as Docker container

---

## Technologies used

- Docker  
- Nexus  
- DigitalOcean  
- Linux  

---

## Project Description

- Create and configure Droplet  
- Set up and run Nexus as a Docker container  

----

## Create a new Droplet on DigitalOcean

- Go to **DigitalOcean**. We already have a running droplet, but now we need to create a new one.
- Click to **Add a new Droplet**.
- For the region, select **Frankfurt**.
- Choose the **Basic** plan and select the **8GB** version.
- Create the droplet.
- Once it’s created, click on it and rename it to `droplet-nexus`.
- Go to **Networking** and configure the firewall to ensure the right ports are open for Nexus and Docker.

### Connect to the Droplet and Install Docker

1. Copy the droplet's IP address and connect via terminal:
 ```bash
   ssh root@IP_ADDRESS
```

2. Update the package manager:
```bash
apt update
```

3. Install Docker using snap:
```bash
snap install docker
```

4. Verify Docker installation by typing:
```bash
docker
```

*You should see the Docker commands displayed.*

----

### Find and Run the Official Nexus Docker Image

To find the official Nexus image to install on the server, go to:  
[https://hub.docker.com/](https://hub.docker.com/) in your browser.

Search for `nexus3` and use the official image from **Sonatype**.  

We will need to configure a volume for it so that data persists.

Create a Docker volume:

```bash
docker volume create --name nexus-data
```

Run the Nexus container with the created volume:
```bash
docker run -d -p 8081:8081 --name nexus -v nexus-data:/nexus-data sonatype/nexus3
```

- The container will run **detached**, in the background, using the `-d` flag.
- The `-p 8081:8081` option maps port **8081** of the container to port **8081** on the host, making Nexus accessible on that port.
- The `--name` option assigns the container the name `nexus`.

Install `net-tools` package:
```bash
   apt install net-tools
```

Check that Nexus is running and listening on ports: **netstat -lntp**
Verify that the Nexus Docker container is running: **docker ps**

### Access Nexus and Inspect Docker Container

1. In your browser, connect to Nexus: `DROPLET_IP:8081` 
2. On the terminal, check running Docker containers:
```bash
docker ps
```

3. Access the Nexus container shell:
```bash
docker exec -it CONTAINER_ID /bin/bash
```

4. Find out which user you are logged in as:
```bash
whoami
```

If `nexus` is displayed, it means the user was created successfully.

5. Exit the container shell:
```bash
exit
```

6. To see where data is stored on the server file system, list Docker volumes:
```bash
docker volume ls
```

7. For more information about the `nexus-data` volume:
```bash
docker inspect nexus-data
```

8. To explore volume data on the server:
```bash
ls /var/snap/docker/common/var-lib-docker/volumes/nexus-data/_data
```

You can check the information either on the server or inside the Docker container.




