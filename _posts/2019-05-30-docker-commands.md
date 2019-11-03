---
title:  "Docker 명령어"
excerpt: "Docker 명령어 모음"
categories:
  - docker
tags:
  - docker
  - command
last_modified_at: 2019-11-02T13:23:00-09:00
---

# Docker Command

---

    #docker commit
    docker commit container_name image_name
    #push
    docker login
    docker tag container_name repository/namespace/image_name
    docker push repository/namespace/image_name
    
    #Single Layer
    docker container export CONTAINER_ID > /myImage.tar
    docker import myImage.tar imageName:lates