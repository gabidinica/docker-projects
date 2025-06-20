# Demo Project

## 🚀 Project Overview
This demo project demonstrates how to use **Docker Compose** to run multiple Docker containers seamlessly.

## 🛠️ Technologies Used
- **Docker**  
- **MongoDB**  
- **MongoExpress**

## 📋 Project Description
The goal of this project is to write a Docker Compose file that runs both **MongoDB** and **MongoExpress** containers. This setup allows easy management and visualization of your MongoDB databases through MongoExpress’s web interface.

---

## Clone and Navigate
Clone the project locally and, in your terminal, move into the project folder.

## Docker Compose and mongo.yaml
- You can check the `mongo.yaml` file.
- Docker Compose is a structured way to contain the normal Docker commands.
- The network is not explicitly defined because it’s not needed. Containers communicate using container names. Compose sets up a single network for the app.

## Important Notes
- **Restart: always**  
  This ensures the container restarts automatically if it stops, making sure that `mongoExpress` is always connected to `mongoContainer` when starting the project.

- If both containers start simultaneously, `mongoExpress` might start before `mongoDb` is up and running, causing connection issues.

## 📦 How to Start the Containers
1. In the terminal, move into the `js-app` folder where the `mongo.yaml` file is located:
    ```bash
    docker-compose -f mongo.yaml up
    ```

2. To check the Docker network, open a new terminal and run:
    ```bash
    docker network ls
    ```
    You should see a network named `js-app_default`.

3. To see the running containers:
    ```bash
    docker ps
    ```

## Start the Application
1. Go into the `app` folder in the terminal.
2. Install dependencies:
    ```bash
    npm install
    ```
3. Run the app:
    ```bash
    npm start
    ```

## Access and Setup Mongo-Express
- Open your browser and go to:  
  `http://localhost:8081`  
- Login with your mongo-express credentials.
- Add a new database: `user-account`.
- Click on it and add a new collection named: `users`.

## Test the Application
- Make sure your application is still running.
- Open a new browser tab and go to:  
  `http://localhost:3000`  
- The app should load successfully.
- Click on **Edit profile**, edit the fields, then click **Update Profile**.
- Go back to the mongo-express tab on the **Users** collection. You should see the new entry inserted.

## 🛑 Stopping the Containers
To stop Docker and remove the containers and network:
```bash
docker-compose -f mongo.yaml down
```

- When Docker stops, the network is also deleted. You can verify by running:
```bash
docker network ls
```
