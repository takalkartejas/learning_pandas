# learning_docker

Video link - https://youtube.com/playlist?list=PLeo1K3hjS3uvCeTYTeyfe0-rN5r8zn9rw&si=FuaETf-FVcr-nhBy

---

## Theory
1. 
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
   

  



