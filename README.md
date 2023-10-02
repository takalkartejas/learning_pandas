# learning_docker

Video link - https://youtube.com/playlist?list=PLeo1K3hjS3uvCeTYTeyfe0-rN5r8zn9rw&si=FuaETf-FVcr-nhBy

---

## Theory
1. Pandas is used to manage dataset
---

## Learning steps:
<details> <summary>
1. Basic panda use
   
</summary>

   1. Create newyork weather folder download nyc_weather.csv
   2. Created pandas_intro jupyter file
   3. installed pandas using pip3 install pandas
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
   

  



