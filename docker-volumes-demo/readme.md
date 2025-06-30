# Demo Project

## Persist data with Docker Volumes

### Technologies used
- Docker  
- Node.js  
- MongoDB  

### Project Description
Persist data of a MongoDB container by attaching a Docker volume to it.

## Getting Started

Clone locally the project and in your terminal move into the project folder.

Start MongoDB with Docker Compose
In the console, navigate to your js-app folder and run:
```bash
docker-compose -f docker-compose.yaml up
```

## Check mongo-express

Check that **mongo-express** is running by going to your browser at [localhost:8081](http://localhost:8081) and logging in with the configured credentials.

1. Create a new database: `user-account`.
2. Click on it and create a new collection: `users`.

## Start the application

To start the application, move into the folder where the `server.js` file is located:

```bash
cd app
```

Then install the dependencies: **npm install**
To start the app, run: **node server.js**

## Verify the application

To check that the app is running, go to [localhost:3000](http://localhost:3000), edit the data, then save it.

Go to the other browser tab where **mongo-express** is open and check that the fields are now populated.

## How to use named volumes for data persistence

In the `docker-compose.yaml` file, all the volumes to be used by the containers are listed.

The defined volume is `mongo-data` and it uses the `local` driver.

Once the volume name is defined, it can be used inside the container by mapping it as follows: **mongo-data:/data/db**

## Restart Docker Compose

Restart Docker Compose
To restart Docker Compose, run:
```bash
docker-compose -f docker-compose.yaml down
docker-compose -f docker-compose.yaml up
```

## Verify data persistence

Repeat the above steps to create a new database and collection, and update the profile in the app.

Restart Docker Compose again:

```bash
docker-compose -f docker-compose.yaml down
docker-compose -f docker-compose.yaml up
```

## How to check persisted volumes on your local machine

To access the shell of the Docker VM in order to view volume information, use the following command:

```bash
docker run -it --privileged --pid=host debian nsenter -t 1 -m -u -n -i sh
```

## Exploring Docker volumes

- Type `ls` to check the folders.
- Type `ls /var/lib/docker` to list Docker directories.
- To check the volumes, type:  
  `ls /var/lib/docker/volumes`
- To get to the data that’s replicated on the mongo volume, type:  
  `ls /var/lib/docker/volumes/js-app_mongo-data`  
  `ls /var/lib/docker/volumes/js-app_mongo-data/_data/`

