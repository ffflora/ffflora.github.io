---
title: "Things To Know About Docker"
date: 2022-09-08T16:48:32-07:00
draft: true
toc: false
images:
tags:
  - Notes
  - ETL
  - GCP
categories:
  - Data Science
  - Data Engineering

---



Runs natively on Linux and Windows Server

Not the same as using Virtual Machines 

Key benefits to web developers:

- Accelerate developer onboarding 
- Simplify working with multiple apps 
- Consistency between envs.
- ship faster



## Quick Start

1. Clone a repo

   `docker run --name repo alpine/git clone https://github.com/docker/getting-started.git`

2. Build the image:

   `cd getting-started `
   `docker build -t docker101tutorial .`

3. Run the container 

   `docker run -d -p 80:80 --name docker
   -tutorial docker101tutorial`

4. Save and push the image 

   `docker tag docker101tutorial ffflora
   j/docker101tutorial
   `

   ` docker push fffloraj/docker101tutori`



### Key Docker Machine Commands

```shell 
Docker-machine ls
Docker-machine starts [machine name]
Docker-machine stop  [machine name]
Docker-machine env  [machine name]
Docker-machine ip  [machine name]
```

### Key Docker Client Commands

```shell
docker pull [image name]
docker run [image name]
docker images
docker ps // only shows running containers
docker ps -a // shows all containers
docker rm 598f // delete container with first few id charac
docker rmi 987s // delete image with first few id charac
```

