# Week 1 — App Containerization

To run the Backend application - Python(flask), I had to install all components in the requirements.txt file with the command:

```sh
cd backend-flask/
pip3 install -r requirements.txt
python3 -m flask run --host=0.0.0.0 --port=4567
```
On Gitpod, I opened port 4567 to the public and tested the application with the URL `https://4567-chinedujo-awsbootcampcr-tu0h0msihgu.ws-eu87.gitpod.io/api/activities/home` and got error below:

```
Not Found

The requested URL was not found on the server. If you entered the URL manually please check your spelling and try again.
```

So I stopped the running application and set the required variables found in `app.py` file:

```sh
export FRONTEND_URL="*"
export BACKEND_URL="*"
```
Then restarted the application

```sh
python3 -m flask run --host=0.0.0.0 --port=4567
```
![](images/)

## Containerize Docker Application
I built a [Backend Dockerfile](/backend-flask/Dockerfile) and [Frontend Dockerfile](/frontend-react-js/Dockerfile)

To ensure the preset env variable were not being used by the application, I unset them

```sh
unset FRONTEND_URL
unset BACKEND_URL
```
Built docker file for both frontend and backend applications:

Backend Dockerfile:

```dockerfile
FROM python:3.10-slim-buster

WORKDIR /backend-flask

COPY requirements.txt requirements.txt

RUN pip3 install -r requirements.txt

COPY . .

ENV FLASK_ENV=development

EXPOSE ${PORT}

CMD [ "python3", "-m", "flask", "run", "--host=0.0.0.0", "--port=4567" ]

```

### Backend Docker build and run sequence

To build the docker images, input the following:

```sh
docker build -t backend-flask ./backend-flask
```
Confirm the images has been created with the command:

```sh
docker images
```
To see running containers and all available containers:

```sh
docker ps
docker ps -a
```

To run the build images as container, input the following:

```sh
docker run --rm -p 4567:4567 -e FRONTEND_URL="*" -e BACKEND_URL="*" backend-flask
```

OR break commands into multiple lines:

```sh
docker run --rm \
-p 4567:4567 \
-e FRONTEND_URL="*" \
-e BACKEND_URL="*" \
backend-flask
```
### Frontend Docker build and run sequence
First ensure you are in the right directory then install dependencies.

```sh
cd /frontend-react-js
npm install
```
Frontend Dockerfile content:

*Note I used base image of node:16.19-alpine as 16.18 was not available on DockerHub*

```dockerfile
FROM node:16.19-alpine

WORKDIR /frontend-react-js

COPY . /frontend-react-js

RUN npm install

ENV PORT=3000

EXPOSE ${PORT}

CMD ["npm", "start"]
```

### Starting up applications with docker-compose


```dockerfile
version: "3.8"
services:
  backend-flask:
    environment:
      FRONTEND_URL: "https://3000-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
      BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
    build: ./backend-flask
    ports:
      - "4567:4567"
    volumes:
      - ./backend-flask:/backend-flask
  frontend-react-js:
    environment:
      REACT_APP_BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
    build: ./frontend-react-js
    ports:
      - "3000:3000"
    volumes:
      - ./frontend-react-js:/frontend-react-js
networks: 
  internal-network:
    driver: bridge
    name: cruddur
```

Started up the applications using the command below:

```sh
docker-compose up
```
Result:
![cruddur works](/journal/images/cruddur-docker-compose.png)


## STRETCH HOMEWORK
To ensure all dependencies are installed on launching Gitpod, I added the following to gitpod.yml file

```yaml
- name: Install Frontend and Backend dependencies
    init: |
      cd /workspace/aws-bootcamp-cruddur-2023/backend-flask
      pip3 install -r requirements.txt
      cd /workspace/aws-bootcamp-cruddur-2023/frontend-react-js
      npm i
```

### Push local Docker images to Docker Hub

I built the frontend app only and pushed to Docker Hub to be reusable on remote systems:

The frontend app image was built with the commands below:
```sh
cd frontend-react-js/
docker build -t frontend-react-js:v1 .
```

To push to Docker hub, you first have to login Docker then tag the image correctly to reflect the docker user and repo

Login:

```sh
docker login
```
Prompt to input login credentials and success message:

```
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: nedudev
Password: 
WARNING! Your password will be stored unencrypted in /home/gitpod/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```

** Tag the images as below:
```sh
docker tag frontend-react-js:v1 nedudev/frontend-react-js:1.0
```

**  Push image as below:
```sh
docker push nedudev/frontend-react-js:1.0
```

Output:

```sh
The push refers to repository [docker.io/nedudev/frontend-react-js]
3b8430c53a72: Pushed 
80b568d369dd: Pushed 
747aed1e6526: Pushed 
67246b9bbb93: Mounted from library/node 
a1ce792246f9: Mounted from library/node 
af1fa49a98d8: Mounted from library/node 
7cd52847ad77: Mounted from library/node 
1.0: digest: sha256:968aa7c31498c16cf9e85e3c0b9d08db6ee8e43feae5bff2b6f1088a5e86b6aa size: 1788
```
** logout of docker to remove stored credentials

```sh
docker logout
```

Output:

```
Removing docker login credentials for https://index.docker.io/v1/
```

### Implement Healthcheck on Docker Compose for Frontend application

I learnt to do this via a [docker-compose healthcheck article](https://medium.com/geekculture/how-to-successfully-implement-a-healthcheck-in-docker-compose-efced60bc08e)
Healthcheck was implemented  with the command below:

```dockerfile
frontend-react-js:
    environment:
      REACT_APP_BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
    build: ./frontend-react-js
    ports:
      - "3000:3000"
    volumes:
      - ./frontend-react-js:/frontend-react-js
    #Frontend application Healthcheck code
    healthcheck:
      test: curl --fail https://3000-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}/api/activities/home
      interval: 30s
      timeout: 10s
      retries: 5
      start_period: 30s
```

