# learning_docker

Video link - https://www.youtube.com/watch?v=p28piYY_wv8&t=2869s

---

## Theory
1. Docker is a tool for running applications in an isolated enviornment
2. It is similar to virtual mahine
3. Standard for software deployment
4. If you create a package in docker it is sure to work in any system
5. Containers are an abstraction at the app layer that packages 'code' and 'dependencies' together. Multiple containers can run on the same machine and share the OS kernel with other containers, each running as isolated processes in user space.
6. A container does not require a full os it just shares the underlying os with other containers.
 ![Screenshot from 2022-11-16 17-09-58](https://user-images.githubusercontent.com/67382565/202233330-61015c0e-c85c-44bc-8a1b-0aa59fd2cdc9.png)
7. Image - 
   * Image is a template for creating an enviornment of your choice.
   * Image has everything needed to run your Apps.
   * OS, Software, App Code
8. Container is a running instance of image
9. Volume:- allows sharing data between host and container, and also between containers
10. If we add files and folders in this volume in host - they will show up in containers and vice versa
11. Dockerfile :- allows us to build a new image, contains list of steps on how to create images
12. .dockerignore:- when we are doing ADD . ., it will add all the files in the docker. We can exclude some files by editing .dockerignore
13. Caching and layers:- The image building step might take much time everytime we change something like adding more data in node.js 
14. To avoid this the docker uses cache, this takes less time as same things need not to be done again
15. Tags and version:- allows you to control version, avoids breaking changes (eg. if you build your software using node:latest and the latest version chages from 8 to 9 breaking your code)
16. Docker Registries:- 
   1. server side application that stores and lets you distribute docker images
   2. can use docker registris for CD/CI piplines
   3. Can run parts of our application
   4. We use docker push/pull
![Screenshot from 2022-12-04 21-21-42](https://user-images.githubusercontent.com/67382565/205513783-da636623-0556-4506-80bf-04972a31e7e1.png)

---

## Learning steps:
<details> <summary>
1. Pulling and running docker images
   
</summary>

   1. We go to docker hub and explore images
   2. Here we download ngnix image using :- docker pull nginx
   3. Check the image using :- docker images
   4. Run the image using :- (docker run nginx:latest)  here 'latest' is a tag - can be 1.0, 2.0 etc.
   5. Check the container using the command - (docker ps) in other container
   6. run in detached mode :- refer command 2.1
</details>
<details> <summary>
2. Exploring ports
</summary>

   1. We notice the port name as - '80/tcp' in docker ps
   2. We want to map the port 8080 of our local host to the port 80 of the container :- refer command 8.
   3.  We verify by going on browser and typing localhost:8080
   4.  We map another port to the port 'tcp' refer command 3.1
   5.  we verify by typing both ports on browser
</details><details> <summary>   
3. Managing containers
</summary>

   1.  The old container does not get deleted after stopping, we can do 'docker start <containername/id>' to start the container
   2.  The container will automatically get a random name everytime unless you specify it 
   3.  We can delete all containers at once :- refer cmd 2.8 and 2.9
   4.  we can delete containers forcefully if they are running or some other issue shows up refer cmd 2.7
   5.  We can name a container while starting using cmd 2.10
   6.  We write a big command using all the things mentioned earlier as - docker run --name website -d -p 3000:80 -p 8080:80 nginx:latest :-
       * '-d' for detached mode
       * '-p' for port
       * '--name' for name
       * 'ngnix:latest: image name and tag
   7.  Create the FORMAT variable in bashrc and give it a column format to make it easy to see
   8.  We can use the command (docker ps --format=FORMAT) to see the container info in better format 
</details><details> <summary>
4.  Volumes :- refer theory 9 to 10
</summary>   

1.  Now we have created a folder called website in the directory
2.  Create index.html inside the folder and mount the folder as a volume in /usr/share/nginx/html
3.  We use the command :- docker run --name website -v $(pwd):/usr/share/nginx/html:ro -d -p 8080:80 nginx :- refer cmd 4.1:- dont use the 'ro' flag if you wish to modify the directory, pwd :- gives the address of current directory (e.g.- /home/tejas/study/learning_docker)
4.  We go on browser and do localhost:8080 and see the html page that we created
5.  We do docker exec -it website bash to go inside container
6.  We remove the ro flag while running the container again and we go inside the html folder in address mentioned at 23. and touch a about.html file 
7.  The last step creates a file also at host refer theory 10
8.  search for a theme on bootstrap single page template, download, copy the content and paste in the website folder and delete all old files
9.  run docker and check the website on localhost:8080 
   
</details><details> <summary>   
5. Sharing volumes between containers
</summary>   

 1.  using command :- docker run --name website_2 --volumes-from website -d -p 8000:80 nginx :- refer cmd 4.2
 2.  We create new container called website_2 and mount the volume from the container website into website_2.
 3.  We give it a different port no. and check if the website is running

</details><details> <summary>  
6.  Dockerfile :- refer theory 11
</summary>   

1.  We create a docker file with the base image nginx, we add the files from the website folder in the container adresss :- /usr/share/nginx/html, and this is not a volume mount, its static content  
2.  refer Dockerfile for comments
3.  build the image using :- docker build -t website:latest .
4.  The comments should be placed on newline in Dockerfile
5.  Run the container using new image:-docker run --name website -p 8080:80 -d website:latest
6.  check if website is running
</details><details> <summary>
7. Node.js and express
</summary> 

   1.  Install node.js and express
   2.  Create a dir. called user_service_api and do npm init
   3.  copy the helloworld example from the getting started page of express.js and create index.js
   4.  Run the file and check output in browser
   5.  Modify the file to send a json object and check on browser check commits
   6.  Modify to return json array
   7.  We have created a simple API!
   8.  Now we have to dockerize this api
   9.  Create a Dockerfile in user_service_api folder, refer comments in the file
   10. Run docker using :- docker run --name user-api -d -p 3000:3000 usr-service-api:latest
   11. Express js listens to port 3000 by default, we map port 3000 of host to port 3000 of docker
</details><details> <summary>
8. Dockerignore
</summary>

   1.  .dockerignore:- when we are doing ADD . ., it will add all the files in the docker. We can exclude some files by editing .dockerignore
   2.  In this case we do not need to add node_modules as they will be installed in docker due to 'npm install' commands
   3.  Create the .dockerignore file and delete & create the image again
</details> <details> <summary>   
9.  Caching and layers
</summary>   

1.   The image building step might take much time everytime we change something like adding more data in node.js
2.  To avoid this the docker uses cache, this takes less time as same things need not to be done again 
3.  In our case the 1st 4 steps are :-
     Step 1/6 : FROM node:latest
     ---> c71adfc6ec58
    Step 2/6 : WORKDIR /app
     ---> Using cache
     ---> 5ba5e764db30
    Step 3/6 : ADD . .
     ---> 3c7ac8cfbfeb
    Step 4/6 : RUN npm install -g npm
     ---> Running in 3e88b6855c8f
4.  The 4th step is heavy and  5th step is npm install which is also heavy, these steps are not using cache as the step befor them - ADD . . is changing due to the small change we made in index.js
5.  We need the package.json file for installing the node dependencies. Therefore we will add a json file first then run install and then ADD . ., see commit
6.  build the image then change the index.js file and then build the image again
7.  In first build it will download all dependencies and in second build it will use chace improving the speed
</details><details> <summary>
10.   Alpine  
</summary>  

1.  alpine is small in size and efficent, every image has a alpine tag eg. - alpine linux
2.  Lets try pulling alpine version of node:- docker pull node:lts-alpine :- lts stands for latest
3.  The alpine version is jsut 167 mb and latest version of node is 995 MB
4.  Now we change the base image in our Dockerfile to node:alpine and also in other Dockerfile to nginx:alpine
5.  user-service-api images has reduced to 187 MB from 1GB
</details><details> <summary>
11. Dangling images
</summary>

1.  The old images are overwritten everytime we do docker build for same image, they are known as dangling images
2.  remove dangling images using docker rmi $(docker images -f'dangling=true' -q)
</details><details> <summary>    
12. Tagging and versioning :- refer theory 15
   </summary>
    1. change nginx version in websites's dockerfile from alpine to 1.17.2 - alpine and user-service-api dockerfile to 10.16.1-alpine
    2. Build image and run containers
    3. tag a already existing node:latest image using:- docker tag node:alpine node:1 refer cmd 1.9
    4. modify something, create image and then tag as version 2 instead of 1

 </details><details> <summary>
 12.  Repositories:- refer theory 16
   </summary>

1. sign in on docker hub
2. create a public repository called website
3. Docker command given by website:- docker push takalkartejastt/website:tagname
4. do:- docker tag website:latest takalkartejastt/website:1
5. login using:- docker login
6. docker push takalkartejastt/website:1
7. should add overview so that people can understand the image
8. delete the image from pc
9. Do:- docker pull takalkartejastt/website:1
10. Note that the latest version should be pushed to dockerhub as it will pull the latest version if no tag is used while pulling.
</details><details> <summary>
13.  Docker inspect, log and exec
   </summary>

   1. Run a container and do :- docker inspect [container name/id]
   2. returns a json file:- shows volume mounts, env variables like path and dependcies, image, port mapping,
   3. docker log:- returns log , requests form firefox. also shows messege in console.log from program
</details>

---

## Usefull commands:
### 1. Docker images
   1. **docker pull [imageName]**
   2. **docker images** :- see the list of images
   3.  **docker image rm**: delete image
   4.  **docker rmi**:- delete image
   5.  **docker images -f'dangling=true'** :- gives all dangling images that has tag [none] which means they have been overwritten by other image
   6.  **docker images -f'dangling=true' -q** :- give only the image id of dangling images
   7.  **docker rmi $(docker images -f'dangling=true' -q)** :- delete all dangling images
   8. **docker build -t [name]:[tag] [locationOfDockerFile]** :- build image using DockerFile
   9. **docker tag [image name]:[tag] [image name]:[new tag]**
   
### 2. Docker containers   
   1. **docker run [imagename]:[tag]**
   2. **docker container ls** :- docker ps
   3. **docker run -d [imageName]** :- run in detached mode
   4. **docker stop [containerID/containername]**
   5.  **docker ps -a**:-  to show all the containers alongside that had stopped
   6.  **docker rm [containername/id]**
   7.  **docker rm -f [containerid/name]** : delete forcefully
   8.  **docker ps -aq** :- give only ids of all the containers
   9.  **docker rm $(docker ps -aq)** :- delete the things given by this command
   10. **docker run --name [containerName] [imageName]**:- we can manually name container instead of the random name given by pc
   11. **docker ps --format=FORMAT** :- to see the container info in better format
   12. **docker inspect [container name/id]**:- gives a json file with all container related info 
   13. **docker log [containerName/id]**:- returns log , requests form firefox. also shows messege in console.log from program
 
### 3. Ports
   1. **docker run -d -p 8080:80 [imageName]** :-  80/TCP is the port no. of the container which can be seen in the docker ps, here we map the localhost 8080 to the port 80 of the container
      * We can access the image on webpage by entering localhost:8080
   2. **docker run -d -p 8080:80 -p 3000:80 [imageName]** :- can map multiple ports

### 4. Volumes
   1.  **docker run -v [addressOfTheFolderInHOST]:[addressOfTheFolderInDocker] [imageName]** - mount the volume while running the images
   2.  **docker run --name [toBeCreatedContainerName] --volumes-from [containerNameWhoseVolumeWeNeed]  [image_name]** :- to mount the volumes from one container to other 

### 5. Others   
   1.  **docker exec -it website bash**:- to execute in interactive mode
   2.  **pwd** :- gives the address of current directory (e.g.- /home/tejas/study/learning_docker) 

### 6. Docker hub
   1. **docker login**
  



