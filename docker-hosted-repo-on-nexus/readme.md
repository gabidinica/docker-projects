# Demo Project

## Create Docker repository on Nexus and push to it

---

## Technologies used

- Docker  
- Nexus  
- DigitalOcean  
- Linux  

---

## Project Description

- Create Docker hosted repository on Nexus  
- Create Docker repository role on Nexus  
- Configure Nexus, DigitalOcean Droplet and Docker to be able to push to Docker repository  
- Build and Push Docker image  

## Pre-requisites

You need an account on DigitalOcean and create a droplet. On the droplet, install Java version 17 and Nexus.  
Get the droplet IP address and paste it into your browser, followed by port 8081: 'DROPLET_IP_ADDRESS:8081'.
Log in to Nexus with the Nexus user that’s an admin. Also, create a blob store that’s a file and name it `mystore`.

## Create Docker Repository on Nexus

In the Nexus interface, navigate to **Administration → Repositories** and click **Create repository**.  
From the list, select **docker (hosted)**.  

- Name it: `docker-hosted`  
- For Blob Store, select: `mystore`  

Click **Create repository** at the end of the page, leaving everything else as default.  

Once created, click on the newly created repository to find the URL where you can push Docker images.

---

## Create user role for Docker repo

We need to create a new role that has access to the Docker repository.

1. Go to **Security → Roles → Create Role** and select:
   - **Role Type**: Nexus Role  
   - **Role ID**: `nx-docker`  
   - **Role Name**: `nx-docker`  
   - In the **Privileges** section, search for `docker` and select:  
     ```
     nx-repository-view-docker-docker-hosted-*
     ```

2. Click **Create role**.

3. Then, go to **Security → Users**, add a new user, and assign the newly created `nx-docker` role to it.

This user will be used to log in to Nexus using the `docker login` command.

---

## Configure Nexus, DigitalOcean Droplet, and Docker to push to Docker repository

1. In Nexus, go to **Repositories**, click on your `docker-hosted` repository, and click **Edit**.

2. Under the **HTTP** section, enable the checkbox to edit the HTTP port field and set it to: 8083.

3. Save the changes.

4. On your terminal, connect to the DigitalOcean droplet:
```bash
ssh root@DROPLET_IP_ADDRESS
```

5. Verify that the new port is listening by running:
```bash
netstat -lntp
```

You should see output similar to: *0 :::8083*

### Configure Firewall and Enable Docker Bearer Token in Nexus

We need to go to **DigitalOcean** and add the port to the firewall.

- Navigate to **Networking → Firewalls**.
- In the **Inbound Rules** section, add a new rule:
  - **Type**: Custom TCP  
  - **Port**: 8083  
- Click **Save**.

This rule will allow us to communicate with the Docker repository.

---

Go back to the Nexus window and click on **Security → Realms**.

- In the **Available** column, locate **Docker Bearer Token Realm**.
- Click the **+** button in front of it so it gets activated.
- Click **Save**.

This will configure the issuing of the Docker token, which is required for Docker clients to authenticate.

----

### Configure Docker client for insecure registry

Because Nexus runs on a non-secure registry (HTTP), we need to configure the Docker client to allow our `docker-hosted` repo as an insecure registry.

1. Open **Docker Desktop UI** and go to **Settings**.

2. Click on **Docker Engine**.

3. After the `"experimental"` line, add the following configuration block (inside the existing JSON):
   ```json
   {
     "insecure-registries": ["IP_ADDRESS_OF_NEXUS:8083"]
   }
 ```

4. Replace `IP_ADDRESS_OF_NEXUS` with the actual IP address of your Nexus server.

5. Replace the port number with the one configured for your Docker repository (in this case, `8083`).

6. Click **Apply & Restart** in Docker Desktop to apply the changes.

This will allow the Docker client to connect and push images to your Nexus Docker repository over an insecure HTTP connection.

---

### Docker Login

Use the following command to log in to your Docker registry:

```bash
docker login registry_endpoint_IP_ADDRESS:8083
```

You’ll need to pass in the username on the previously created user and its password. If successful you’ll see **Login Succeeded.**

### Push Image to Nexus repo

The first step is to build the image.  
In the terminal, navigate to the app folder (e.g., `js-app`) and run:

```bash
docker build -t my-app:1.0 .
```

Next, tag the existing image to include the repository address:
```bash
docker tag my-app:1.0 REPO_IP_ADDRESS:8083/my-app:1.0
```

This tag tells Docker where to push the image to implicitly.

Check the images with the following command:
```bash
docker images | grep my-app
```

To push the image, run:
```bash
docker push REPO_IP_ADDRESS:8083/my-app:1.0
```

To check the image:

- Open your browser and go to Nexus.
- Navigate to **Repositories → docker-hosted**.
- Refresh the page, and you should see the pushed image.

---

### Fetch Docker Image from Nexus

Use the following `curl` command to list components in the `docker-hosted` repository:

```bash
curl -u username:password "http://REPO_IP_ADDRESS:8081/service/rest/v1/components?repository=docker-hosted"
```
