# 🛠️ Use Docker for local development

##Technologies used:

 Docker, Node.js, MongoDB, MongoExpress

##Project Description:
- Create Dockerfile for Nodejs application and build Docker image
- Run Nodejs application in Docker container and connect to
- MongoDB database container locally.

---

## 📁 1. Clone and Navigate to the App

```bash
git clone git@github.com:gabidinica/docker-projects.git
cd js-app/app

## 🚀 2. Start the App

npm install        # Install dependencies
node server.js     # Start the app

If the app runs successfully, you'll see: app listening on port 3000!

## Setting Up MongoDB and Mongo Express with Docker

Open a new terminal window.

To install MongoDB and Mongo Express, go to the Docker Hub website:  
https://hub.docker.com/

1. Search for **mongo**, click on the Official image, then copy and paste into your terminal:  
   ```bash
   docker pull mongo

2. Search for mongo-express, click on the Official image, then copy and paste into your terminal:
  ```bash
  docker pull mongo-express
3. To check the downloaded images, run:
  ```bash
  docker images

