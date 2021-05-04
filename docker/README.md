# Docker

### Docs

[Reference](https://docs.docker.com/engine/reference/run/)

_sample file_
[Dockerfile](Dockerfile)

_sample file_
[Docker Compose](docker-compose.yaml)

## CLI

##### Search docker containers on dockerhub

    docker search

##### Tag docker image for AWS

    docker tag hello-world:latest aws_account_id.dkr.ecr.us-east-1.amazonaws.com/hello-world:latest

##### Upload Docker image to AWS ECR

    docker push aws_account_id.dkr.ecr.us-east-1.amazonaws.com/hello-world:latest

##### Get login details for AWS ECR

    aws ecr get-login-password --region region | docker login --username AWS --password-stdin aws_account_id.dkr.ecr.region.amazonaws.com

##### Pull image from dockerhub

    docker pull ubuntu

- Pull the ubuntu image from dockerhub

##### List images

    docker images ls

##### List containers

    docker container ls -a

- a - List all containers, without the flag it only shows active containers

##### Remove image

    docker rmi image_name or ID number

##### Remove all containers

    docker container prune

#### Start a container

##### Build container from local Dockerfile

        docker build -t image_name .

- t - name you wish to give the container

##### Run container after building it

        docker run -d --name container_name -p 80:80 image_name

- d -
- name - give container name or a default one will be created
- p - assign port mapping first is host second is container port

##### Install docker

    sudo apt install docker
