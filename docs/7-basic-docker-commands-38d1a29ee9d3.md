# 7 个基本 Docker 命令

> 原文：<https://javascript.plainenglish.io/7-basic-docker-commands-38d1a29ee9d3?source=collection_archive---------2----------------------->

## 每个软件都应该知道的 Docker 命令

![](img/1ecb307a7baab13c59f5c215d22b0727.png)

Photo by [Andy Li](https://unsplash.com/@andasta?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/container?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Docker 是一个工具，旨在通过使用容器来简化应用程序的创建、部署和运行。所有开发人员都知道。

这里我列出了一些每个软件开发人员都应该熟悉的基本 docker 命令。

注意:随着时间的推移，这个列表会越来越丰富。如果有任何遗漏，请随时写下评论。

## 1.列出容器

```
docker ps 
# your can add available [OPTIONS] belowdocker ps -aq
# only shows IDs
```

`[OPTIONS]`很少有..更多你可以从[这里](https://docs.docker.com/engine/reference/commandline/ps/)找到

```
**--all** 
# or **-a** Show all containers (default shows just running)**--filter** 
# or **-f** Filter output based on conditions provided**--format** 
# Pretty -print containers using a Go template
```

## 2.停止容器

```
docker stop **CONTAINER_ID** 
# copy container id from **docker ps**docker stop $(docker ps -aq)
# stop all Running containers
```

## 3.移除容器

```
docker rm **CONTAINER_ID** # Delete Specific containerdocker rm $(docker ps -aq)
# Remove all container
```

## 4.列表图像

```
docker image ls 
# List all available images in your machine.docker image ls -aq
# list image ids only
```

## 5.移除图像

```
docker image rm IMAGE_ID
# Remove specific image docker image rm -f IMAGE_ID
# force remove including dependent child images
```

## 6.构建图像

```
docker image build -t **IMAGE_NAME**:**BUILD_NUMBER** **.** # build image based on docker file in Current directory (.)
```

## 7.运行图像

```
# Command format -> docker run [OPTIONS] IMAGE [COMMAND] [ARG...] docker run -d --name **YourContainerName** -p x:y \ # -d will Run container in background and print container ID 
# Docker run on image in port x
# EXPOSE Y from Docker file-e REACT_APP_SOME_ENVIRONMENT = someEnv \
# Set some environment variableIMAGE_NAME:BUILD_NUMBER
# Your image with build number
```

## 8.运行容器内的运行命令

```
docker exec -it **CONTAINER_NAME** sh
# Now you can enter command in the containerExit
# For getting Out from the docker Container
```

# 感谢阅读！🍻