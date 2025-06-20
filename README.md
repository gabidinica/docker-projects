# 🛠️ Use Docker for local development

## Technologies used:

 Docker, Node.js, MongoDB, MongoExpress

## Project Description:
- Create Dockerfile for Nodejs application and build Docker image
- Run Nodejs application in Docker container and connect to
- MongoDB database container locally.

---

## 📁 1. Clone and Navigate to the App

```bash
git clone git@github.com:gabidinica/docker-projects.git
cd js-app/app
```

## 🚀 2. Start the App

- npm install        > *Install dependencies*
- node server.js     > *Start the app*

If the app runs successfully, you'll see: app listening on port 3000!

## Setting Up MongoDB and Mongo Express with Docker


To install MongoDB and Mongo Express, go to the Docker Hub website:  
https://hub.docker.com/

1. Search for **mongo**, click on the Official image, then copy and paste into your terminal on your computer:  
   ```bash
   docker pull mongo
   ```

2. Search for mongo-express, click on the Official image, then copy and paste into your terminal:
  ```bash
  docker pull mongo-express
  ```

3. To check the downloaded images, run:
  ```bash
  docker images
  ```

### Create a Docker Network

We need to create a Docker network to connect **mongo-express** and **mongodb**:
   ```bash
   docker network create mongo-network
```

To see existing Docker networks, run:
```bash
 docker network ls
```

### Create a MongoDB Container

Run the following command to start a MongoDB container:
```bash
docker run -d \
  -p 27017:27017 \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=password \
  --name mongodb \
  --net mongo-network \
  mongo
```

The default MongoDB port is *27017*.

To check if the container is running and view logs, run:
```bash
docker logs <container_id>
```

### Create a Mongo Express Container

Run the following command to start a Mongo Express container:
```bash
docker run -d \
  -p 8081:8081 \
  -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
  -e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
  -e ME_CONFIG_BASICAUTH_USERNAME=user \
  -e ME_CONFIG_BASICAUTH_PASSWORD=pass \
  --net mongo-network \
  --name mongo-express \
  -e ME_CONFIG_MONGODB_SERVER=mongodb \
  -e ME_CONFIG_MONGODB_URL=mongodb://mongodb:27017 \
  mongo-express
```

A default admin user account is created when the *mongo-express* container is started; use this to log in to the Mongo Express UI.

To check if the container is running and view logs, run:
```bash
docker logs <container_id_or_name>
```
> *ME_CONFIG_MONGODB_URL ensures proper connection between Mongo Express and your MongoDB container on the Docker network.*

The connection between node.js and mongdb, can be checked into *server* file.

*Note!* You shouldn’t have the password or root/admin user, set into mongoUrlLocal variable, it’s only for purpose of the project here.
Those are the ones set in the environment variables when we created the docker mongo db container.

### Using Mongo Express and Testing the Application

1. Open your browser and go to: *localhost://8081*
2. Log in with your Mongo Express credentials.
3. Add a new database named: *user-account*.
4. Click on the new database and add a collection named: *users*.

Make sure your Node.js application is still running, then open a new browser tab and go to:
*http://localhost:3000*
The app should load successfully.

- Click on Edit Profile, modify some fields, then click Update Profile.
- Go back to the browser tab with Mongo Express, and under the users collection, you should see a new entry inserted.

### Viewing MongoDB Logs

To view the logs of the MongoDB container, especially the latest lines, run:
```bash
docker logs <container_id_or_name> | tail
```

To stream logs in real-time, run:
```bash
docker logs <container_id_or_name> -f
```
