# chat-app-react.

Snappy is chat application build with the power of MERN Stack. You can find the tutorial [here](https://www.youtube.com/watch?v=otaQKODEUFs)


![login page](./images/snappy_login.png)

![home page](./images/snappy.png)


This is a 3-tier chat application deployed on docker with docker networking best practices considered while working
with Docker and deployed using docker-compose to  build multiple images and run multiple docker containers..

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
#save the file.

```

create Dockerfile for frontend. the frontend is named public in this application. change directory to public.
put all environmental variables in the dockerfile. 
to see environmental variables, check the .env file in each directories for frontend and backend to see their env.

```
cd public
vi Dockerfile
#create your Dockerfile for frontend and copy into apache2 server and expose port 80 for apache2

```
create Dockerfile for backend. the backend is named server in this application. change directory to server

```
cd server
vi Dockerfile
#create your Dockerfile for the backend

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


# BUILDING AND DEPLOYING THIS APPLICATION USING A PIPELINE IN JENKINS AS CI/CD TOOLS

To deploy this application, we are going to take security into consideration while pushing our image to dockerhub. a credential will be created to achieve this security best practise in securing our pipeline script. 

create a Dockerfile and docker-compose.yml for your jenkins image and container. 
refer to this link and git clone to your environment. [please click here for the jenkins build repo](https://github.com/akurasy/jenkins-build.git) . run the below command to create your jenkins server

```
docker-compose up -d
```

# login to your jenkins server using publicIP:8080

Create a jenkins pipeline job by use the url of this repo as your source code

you only create github credentials if you are git cloning from a private repo. we will limit this practise to a public repo.

build the application. 

# after building the application, you will notice that the docker-compose push command will fail, this is because we have not created any docker repo with that image name. we are going to do these two things on dockerhub:

  1. goto dockerhub, create a repo for the frontend and backend. mine was named frontend and backend respectively. please note this           image name must be the same on your docker-compose.yaml file. my images were name akurasy/frontend and akurasy/backend
     so, the repo will appear in this format akurasy/frontend and akurasy/backend . akurasy is my dockerhub username.
     
  2. create a credential to be used by jenkins to perform a read,write,execute command on your dockerhub.
     goto account ------ security ----- add token and create a token. copy and save the token created


# now on jenkins server, we want to create a credential for our docker token and use that credentials to login via jenkins:

goto jenkins dashboard-------- click on your username e.g admin ----- click on credentials ----- system ---- global credentials --- add credentials---- add dockerhub username and use the token generated as password and save. this will generate a credential. 

# now we need to create a global properties (environmetal vriables which provides security to our created credentials)
goto manage jenkins ---- system -- scroll to global properties ----- select environmantall vriables ---- enter variable details
for the above in this practise our environmental variable for jenkins-docker integration has been defined inside our jenkinsfile in this repo, goto jenkinsfile on line 23 we have the variable written as   withCredentials([usernamePassword(credentialsId: "${DOCKER_REGISTRY_CREDS}". our vriable name is "DOCKER_REGISTRY_CREDS"
use this name to configure your global properties and use the credentials ID created initially as the value. then save.

build your application again and the image will be pushed to your dockerhub using the env variable created for docker.
goto dockerhub and confirm that images were successfully pushed or check the console output for job details. 

# PLEASE NOTE this job on this CI/CD can also be done using a github action. the practise for github action will be done in another session. 
