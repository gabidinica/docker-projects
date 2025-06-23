# 🚀 Demo Project: Deploy Docker Application on a Server Using Docker Compose

## 🛠️ Technologies Used
- Docker  
- Docker Compose  
- Amazon ECR  
- Node.js  
- MongoDB  
- MongoExpress  

---

## 📌 Project Description

- Copy `docker-compose` file to a **remote server**
- Log in to **private Docker registry** (Amazon ECR) on the remote server
- Start our application container along with **MongoDB** and **MongoExpress** using Docker Compose

---

## 📁 Steps to Deploy

### 1. Clone the Project

```bash
git clone <your-repo-url>
```

### 2. Open Project in Visual Studio Code
- Locate the file: mongo.yaml

- You’ll find a new container defined: my-app

- It contains the image URL pointing to your AWS ECR private repository
- App will run on port **3000**, mapped to host port **3000**
> ⚠️ The remote server must be authenticated to pull from the **private ECR repository**.

### 🔄 Code Adjustment for Docker Communication

Open server.js file.
We need to replace `mongoUrlLocal` with `mongoUrlDockerCompose`, because when we connect an app that’s in a container with another app that’s in another container we won’t use the localhost anymore.

## 🔨 Build and Push Docker Image
### 1. Rebuild Image with Updated Code
```bash
docker build -t my-app:1.0 .
```

2. Tag the Image
```bash
docker tag my-app:1.0 EXAMPLE_ID.dkr.ecr.eu-central-1.amazonaws.com/my-app:1.0
```

3. Push the Image to AWS ECR
```bash
docker push EXAMPLE_ID.dkr.ecr.eu-central-1.amazonaws.com/my-app:1.0
```

## 🚢 Deploy App with Docker Compose

1. Log In to AWS ECR on the Server
In the browser, go to your ECR repository:
- Select your repo → Click View push commands
- Copy the Step 1 login command:
```bash
aws ecr get-login-password --region eu-central-1 | docker login --username AWS --password-stdin EXAMPLE_ID.dkr.ecr.eu-central-1.amazonaws.com
```

> ✅ You should see: `Login Succeeded`

2. Copy Compose File to the Server
Open terminal and create the compose file using `vim`:
```bash
vim mongo.yaml
```

3. Start All Containers
```bash
docker-compose -f mongo.yaml up
```

## 🌐 Verify Application

1. Open MongoExpress
Navigate to:
http://localhost:8081
- Log in using your MongoExpress credentials
- Create a new database: user-account
- Add a new collection: users

2. Open Node.js App
In a new browser tab, go to:
http://localhost:3000

> You should see the application loaded successfully!

## ✅ Done!
You've now deployed a full Node.js + MongoDB app using Docker Compose and AWS ECR on a remote server!
