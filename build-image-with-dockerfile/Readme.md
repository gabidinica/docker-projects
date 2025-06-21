# 🐳 Dockerfile Guide – Building a Node.js Docker Image

## 📄 What is a Dockerfile?

A **Dockerfile** is a set of instructions for building a Docker image.  
Every image we build is based on an **already existing image**.

---

## 🔍 Choosing a Base Image

1. Open your browser and search for **[Docker Hub](https://hub.docker.com)**.
2. Search for `node` and click on the **official Docker image**.
3. In our case, we use the **`node:20-alpine`** version instead of the `latest` one.

> ℹ️ It's recommended to manage **environment variables** in a `docker-compose.yaml` file so you don't need to rebuild the image every time a variable changes.

---

## 📥 Cloning the Project

- Clone the project locally.
- You’ll find the `Dockerfile` in the `js-app` folder.
- The Dockerfile must be named **`Dockerfile`** – it **cannot** be renamed.

---

## 🏗️ Building the Docker Image

1. Open a terminal and navigate to the `js-app` folder:
```bash
cd js-app
```

2. Build the image using:
```bash
docker build -t my-app:1.0 .
```

> 🏷️ `-t my-app:1.0`: adaugă un tag personalizat imaginii Docker.  
> 📁 `.` indică directorul curent, unde se află fișierul `Dockerfile`.

3. Verify that the image was created:
```bash
docker images
```

## 🚀 Running the Container
To run and test the image:
```bash
docker run my-app:1.0
```

If successful, you should see: `app listening on port 3000!`

### ⚠️ Note
Whenever you adjust the Dockerfile, you must rebuild the image.

## 🧹 Cleaning Up
Delete the Docker Image:
```bash
docker rmi IMAGE_ID
```

If you get an error saying the image is in use:

1. Check the containers using:
```bash
docker ps -a | grep my-app
```

2. Stop the container:
```bash
docker stop CONTAINER_ID
```

3. Remove the container: 
```bash
docker rm CONTAINER_ID
```

4. Now try removing the image again:
```bash
docker rmi IMAGE_ID
```

## ♻️ Rebuilding and Verifying

1. Rebuild the image:
```bash
docker build -t my-app:1.0 .
```

2. Check the image:
```bash
docker images
```

3. Run the image:
```bash
docker run my-app:1.0
```

4. Check that the app is running:
```bash
docker ps
```

5. View logs:
```bash
docker logs CONTAINER_ID
```

## 🖥️ Accessing the Container Shell
To open a terminal inside the container:
```bash
docker exec -it CONTAINER_ID /bin/sh
```

To check the environment variables that we set in Dockerfile, type:
```bash
env
```
> **In the results, you'll see: MONGO_DB_USERNAME and MONGO_DB_PWD**

To check the created folder:
```bash
ls /home/app
```

Exit the container shell:
```bash
exit
```
