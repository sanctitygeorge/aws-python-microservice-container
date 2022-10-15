[![Python application test with Github Actions](https://github.com/sanctitygeorge/aws-python-microservice-container/actions/workflows/pythonapp.yml/badge.svg)](https://github.com/sanctitygeorge/aws-python-microservice-container/actions/workflows/pythonapp.yml)

## Overview

The main aim of this project is to operationalize a Python App from a foundational basis. This simple app uses the fast-api to produce Wikipedia results for any key phrase. This project could be extended to any other type of APIs microoservices, machine learning model like prediction


## Scaffold

1. Create a Python Virtual Environment `python3 -m venv ~/.venv' or 'virtualenv ~/.venv`
2. Create empty files: `Makefile`, `requirements.txt`, `main.py`, `Dockerfile`, `mylib/__init__.py`
3. Populate Makefile
4. Setup Continuous Integration, i.e. check code for issues like lint errors
5. Build cli using Python Fire library `./cli-fire.py --help' to test logic`

## Continuous Integration

1. Setup Github actions for CI whenever there a push to the repository. An example is on the
     
     `.github/workflows/pythonapp.yml`
 
 Example of a failed action: 
 
      ![image](https://user-images.githubusercontent.com/40411079/195979030-815cebf8-ae33-4a2d-bae1-05d2e1c803d0.png)


## Deploy to Docker Desktop

Docker Desktop is an easy-to-install application for your Mac, Linux, or Windows environment that enables you to build and share containerized applications and microservices.

1 You can install Docker Deskstop locally by dowloading the exe file at https://docs.docker.com/desktop/install/windows-install/. 

Note: You need to have a docker account to be able to containerized your application. To sign-up, please visit the portal https://hub.docker.com/signup 

2. Create a Docker File to specify the working directory of the fast-api microservice application and the ports

3. Execute the build command to build a containarized application
      
      `docker build -t <image name> .`

4. Run the python app in Docker Container as below:
 
 `docker run -p 127.0.0.1:8080:8080 <Dockerimage ID>`

5. Test the application 

### Push container to docker hub

To push the container to docker hub, you need to name your local images using one of these methods:

When you build them, using `docker build -t <hub-user>/<repo-name>[:<tag>]`
By re-tagging an existing local image `docker tag <existing-image> <hub-user>/<repo-name>[:<tag>]`
By using `docker commit <existing-container> <hub-user>/<repo-name>[:<tag>]` to commit changes
Now you can push this repository to the registry designated by its name or tag using the command.

 `docker push <hub-user>/<repo-name>:<tag>`

## Continous Delivery of Contanerized Paas Microservice

After the microservice is deployed to dockerhub, we also deployed the application to AWS Elastic Container Registry (ECR) for future use by developers. We need to do the following: 

1. Create a programatic IAM User with access key and secret in the console or create a normal user with username and password. Ensure you assign an adminstrator role/policy to this user

2. Create a new ECR in the AWS console and copy the name for future use. 

3. Run the following commands which can alo be found in the 'Makefile' of this repository:

       `aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <ECR Address or name>`
	   `docker build -t <ecr repository name>.`
	   `docker tag <ecr repository name>':[tag] <ECR Address or name>/<ecr repository name>:[tag]`
	   `docker push <ECR Address or name>/<ecr repository name>':[tag]`

If you have clone this repoitory, you can just run the 'Make deploy' command from the CLI and the microservice container will be deployed to ECR. 

![image](https://user-images.githubusercontent.com/40411079/195979104-55720dad-415b-4b71-bb55-264e1b4a8389.png)


Note: To avoid accrued credits on your accouunt, ensure you delete all the AWS created for this project after successfully testing all the components of the code. 
