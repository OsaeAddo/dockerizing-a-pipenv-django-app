## Dockerizing a Django app(with pipenv)


- ## Docker Instructions
``` 
	From --> For build the docker image, From instraction must needed, because it will take the information from base image (OS base, other software package images) where to start building docker image.
	
	WORKDIR --> It is used for switch to that directory for copy or add files or run some command in that directory in container. It also helps to create the new directory, which are used for building the image, copy the code and settings to it.
	
 	ENV --> It's use for environment variable of docker images. Docker image contains the lightweight operating system. we can push and override the environment variable from  calling it 
 	
 	COPY --> Helps to copy the file from host machine to docker container.
 	
 	ADD --> Helps to copy the file from host machine to docker image or container. It as like as copy instractions, it has extra functionality, it copy the file from remote also
 	
 	RUN --> Run the command inside the docker image at image building time. It support valid operating system command.   executes command(s) in a new layer and creates a new image 
 	
 	CMD -->  sets default command and/or parameters, which can be overwritten from command line when docker container runs 
 	
 	ENTRYPOINT -->  used for configure the container that will run as an executable. We can add the multiple command shell file .sh file in entrypoint instructions. it will run 
 	
 	EXPOSE --> It will help to expose the port for to in docker to access by other container or images. we also mapping with host computer port during run the docker container.
```

- ### Format for writing in Dockerfile
``` 
	#Comment
	INSTRUCTIONS argument
```

1. ## Add a 'Dockerfile' to the root of your project.
	``` 
	$ touch Dockerfile 
	```


2. ## Add the following to your Dockerfile:
- (This isn't a strict approach to building your Dockerfile, a understand the instructions above & you can configure your it however you want)
```
	# For more information, please refer to https://aka.ms/vscode-docker-python
	FROM python:3.9.0-buster
	
	
	# Keeps Python from generating .pyc files in the container
	ENV PYTHONDONTWRITEBYTECODE=1
	
	# Turns off buffering for easier container logging
	ENV PYTHONUNBUFFERED=1
	
	ENV PROJECT_DIR /Lechateau-resort
	
	WORKDIR ${PROJECT_DIR}
	
	# Copy all files in local root directory to root of docker image
	COPY . .
	
	
	RUN pip3 install pipenv
	
	
	RUN pipenv shell
	
	# Install all packages from Pipfile
	RUN pipenv install
	
	
	EXPOSE 8000	
```

3. ## Add a 'docker-compose.yml' file to the root of the directory
```
	touch docker-compose.yml
```
4. In the docker-compose.yml, add the following:
- (Again this can be customized based on your project setup/requirements)
```
	version: '3.4'
	
	services:
	  lechateauresort:
	    image: lechateauresort
	    build:
	      context: .
	      dockerfile: ./Dockerfile
	    command: python manage.py runserver 0.0.0.0:8000
	    ports:
	      - 8000:8000
```
## This next few steps should ideally be done after docker installation. 
### You have to create a 'docker' group & add your user. Failure to do so, & you might probably run into the '[err no. 13]: Permission denied' error when running any docker command in the cli (e.g docker build, docker pull).

## Post-installation of docker
- Create the docker group
```
	sudo groupadd docker
```

- Add your user to the group
```
	sudo usermod -aG docker $USER
```

- Activate the changes to the groups(for linux)
```
	newgrp docker
```

- Test if you can run docker commands without sudo
	- If it worked, a message will be printed & exit after.
```
	docker run hello-world
```

### Read more from the docker docs [https://docs.docker.com/engine/install/linux-postinstall/]