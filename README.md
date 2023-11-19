# chat-app-react-

Snappy is chat application build with the power of MERN Stack. You can find the tutorial [here](https://www.youtube.com/watch?v=otaQKODEUFs)


![login page](./images/snappy_login.png)

![home page](./images/snappy.png)




This is a 3-tier chat application deployed on docker with docker networking best practices considered while working
with Docker and deployed using docker-compose to  build multiple images and run multiple docker containers.

The application is composed of a frontend built with reacts and backend with nodejs which writes into a mongo database.

To deploy this application, ensure that you have the requirements in the table below installed on the host machine before deploying the application.

|Installation|Required |
| ------------- | ------------- |
| Docker Installed  | :heavy_check_mark:  |
| Docker-compose installed | :heavy_check_mark:  |

to check docker and doker-compose installation, run 

```
docker --version
docker-compose --version

```

Update your server and create a working directory. Pls note we will be deploying this application on ubuntu server


```
sudo apt update -y
mkdir Project
cd Project
```

Initialize git in the working directory and clone the repo.

```
git init
git clone https://github.com/akurasy/chat-app-react-nodejs.git
cd chat-app-react-nodejs
```

edit the API file in thE public/src/utils directory and replace localhost with your public IP. leave as localhost if only you will be testing on the
local machine used to deploy. 

```
vi public/src/utils/APIRoutes.js
# change line 1: export const host = "localhost:5000";
# replace localhost with public IP: export const host = "publicIP:5000";
#save the file

```

create Dockerfile for frontend. the frontend is named public in this application. change directory to public.
put all environmental variables in the dockerfile. 
to see environmental variables, check the .env file in each directories for frontend and backend to see their env.

```
cd public
vi Dockerfile
#create your Dockerfile for frontend and copy into apache2 server and expose port 80 for apache2.

```
create Dockerfile for backend. the backend is named server in this application. change directory to server

```
cd server
vi Dockerfile
#create your Dockerfile for the frontend

```

change directory back the working directory (chat-app-react-nodejs) to create a docker-compose file.
docker-compose is used to build and run multiple containers at once.

```
vi docker-compose.yml
#create your docker-compose file

```
after creating the docker-compose file, save and exit.

To deploy the application, run the command

```
docker-compose up -d

```

to see the state of the container, run the command
# docker logs cointainer-name

to see built images and running containers, run 

```
docker images #to see built image
docker ps #to see running containers

```
you can view your application on your brower by using your public IP

# paste your public IP on your browser to see your deployed application
example: http://34.45.162.23

no need to add the exposed port 80 since it is a default port for your apache web server and this is the port for your local browser

you can change port in your dockerfile and docker-compose file for frontend to deploy on development environment.
you do not need to copy to apache server while in the dev environment.
