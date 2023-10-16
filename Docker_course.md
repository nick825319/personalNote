# docker

resource, practice, github repo:  
https://github.com/bretfisher/udemy-docker-mastery  


## Linux on Windows - WSL2 (Windows subsystem linux)
WSL2 - windows 內建linux系統  
windows terminal: 整合cmd, powershell, linux OS cmd, other os 的工具  
WSL2 可添加至windows terminal設定

## docker client (Docker Desktop)
Docker Desktop have license (only free for learn and personal)
```
Docker Desktop can deploy and run Docker containers on your local machine, as well as connect to remote Docker engines or Docker Swarm clusters.
```

Docker download of an image, so you can run containers is called a pull, kind of like a Git pull.
 
This requires access to the Docker Hub.

(CPU / BIOS virtualization needs open 需要CPU虛擬化，在BIOS/UEFI設定)  

windwos, MacOS, 需要虛擬化，linux不需要，Docker是基於LINUX發展出來的。


### local linux clinet cmd
\$apt-get install docker-ce-cli

### local windows clinet cmd
\$choco install docker-ce-cli

registry follow doc in web

## docker engine (server)
docker engine - open source (free)

docker engine: minimun docker deamon on seerver side  

## image v.s. container
`Image` is the binaries and libraries and source code that all make up your application  
`container` is a running instance of that image.

container 只是單純的processes (in linux)，在$ps aux中可以看到docker正在run的app process. 

# Docker command
get image to local
```
docker pull 
```
list image in local
```
docker image ls
```

## docker informance
docker command line structure:  
```
old (still work): docker <command> (options)  
new: docker <command> <sub-command> (option)
```


--- 
### Info
engine version
```
docker version
```
most config value of engine
```
docker info
```


---
### container

start a new container from an image  
docker engine looked for image, pulled from Docker hub, started a new process in a new container 
```
(old) docker run 
docker container run --publish 80:80 nginx
```

run in background
```
docker container run --publish 80:80 --detach nginx
docker container run --publish 80:80 --d nginx
```

add container name (--name)
```
docker container run --publish 80:80 --detach --name webhost nginx
```


stop container (in Windows, need stop. If in Linux, can just ctrl+c)  
container id can use front unqie number(e.g. 69052154ce -> 690)
```
docker container stop <container id/name>
docker stop <container id/name>
```

docker ps  
list running container
```
docker ps
```

docker top  
list running processes in specific container
```
docker top <name>
```
details of one container config, matadata (startup, config, volumes, network)
```
docker container inspect
```
performance stats for all containers
```
docker container stats
```

remove (delete) one or more containers  
-f : force remove
```
docker container rm <containerId ...>
docker container rm -f <containerId ...>
docker rm <containerId ...>
```

list running containers
```
docker container ls
(old) docker ps
```

show logs for specific container (--help)
```
docker container logs
docker logs 
```

### shell inside containers
docker container run --help  
container run 後方的參數格式:  
Usage: docker container run [options] IMAGE [command] [arg]  

現在要使docker container 可以使用shell進行交互
---
-i: interactive mode   
-t: tty (teletype, allocate a pseudo-tty 字符傳輸)
```
docker container run -it --name proxy nginx bash 
---
以下代表已進入該container root交互環境，此處相似於linux (ls -al)
root@<containerID>:/#
---
離開:
exit
以此方式離開，container會關閉，因使用bash啟動生命週期proxy(nginx)與bash一致。
```

回到停止的container (exit, stop. docker container ls -a 可以看到該關掉的container):  
-i:interactive  -a: attach STDOUT/STDERR, dorward signals
```
docker container start -ai
```

`exec`: run a command in a running container:  
exit後container依舊存在，exec runs an additional process
```
docker container exec -it <name> bash
```


## docker networking

```
docker container run -p 80:80 --name webhost -d nginx
```
It shows the ip addresses which ports are forwarding traffic to container from host
```
docker container port webhost
(for command last one: it show 80/tcp -> 0.0.0.0:80)
```
--format: show the filtering result of actual node of JSON output
```
docker container inspect --format '{{ .NetworkSetting.IPAddress }}' webhost
```

* 若不指定 port "-p" 則無法與外部網路溝通，但不指定port仍可以與container內部app溝通  (-p 80:80  -> -p "localPort (host port)":"remotePort (container)" )
* one container對應 one port，不可 multiple container對應one port


### network cmd
list all network which is NAT'ed behind host IP
```
docker network ls
```
```
docker network inspect [name]
```

create virtual network (default driver: bridge / option: --driver)  
network driver: built-in or 3rd party extensions that give you virtual network features
```
docker network create <network name>
```

In this way, this new nginx will be added to \<network name\>  
inspect \<network name\> will see it.
```
docker container run -d --name new_nginx --network <network name> nginx
```

connect/disconnect exist container to exist network
```
docker network connect <container> <network name>

docker network disconnect <container> <network name>
```

* docker 內建(build-in) DNS resolve，這樣使訊息溝通時可以直接指定container name。使以下命令可執行
```
docker container exec -it my_nginx ping new_nginx
```
* 注意 build-in DNS 只在user defined networks與其他。在default bridge network 沒有此功能，若添加container沒有指定network會被添加在default bridge造成無法方便溝通，需使用 (Legacy)  --link 設定在default bridge。
* 故推薦使用user defined networks


## docker DNS round robin
建立network
```
docker network create dudo
```
--net-alias: 指定此虛擬網路的別名(search)，在dns搜尋時能指定
```
docker container run -d --net dude --net-alias search elasticsearch:2
```

搜尋search此網域名子在目前網路的位置，(一次性使用alpine搜尋一次)
```
docker container run --rm --net dude alpine nslookup search
```

使用curl call search domain name的port 9200。
```
docker container run --rm --net dude centos curl -s search:9200
```
在建立兩個docker後(--net-alias search elasticsearch:2)，此兩的容器有相同別名，curl -s search:9200 會使用DNS round robin，call到隨機一個容器。
![alt text](dnsRR.PNG)


## docker image 
list images
```
docker image ls
```

Docker remains static until you explicitly update it (upll image)

pull image
specify version 1.11.9
```
docker pull nginx:1.11.9
```

specify version 1.11.x, the last versoin in 1.11 series
```
docker pull nginx:1.11
```

specify version 1.x
```
docker pull nginx:1
```

### docker layer
docker layer 指 docker 建立時是以類似stack的方式一層一層疊加file, application, 這些層及可以以不重複的方式被組合成新的image，container本質是在imgae最上層layer加上一層read, write layer再執行。當內部檔案系統(layer struction)經過修改，添加，就會是一個新的image。  
若同一個image，不同容器(container A, B, C)下被修改了檔案系統，才會以copy-on-write的方式複製一份再去修改，以達到docker節省空間的目的。若沒動到檔案系統則是read-only。

常變更的layer(RUN line)放在末端，有可能避免變更層影響不變層導致image rebuild。(like custom web page)


### dockerFile
![Alt text](ex_dockerfile.png)

### stanza
RUN中的command改變就會形成一層layer  
ENV (key value)的參數變化在matadata中，則不會形成layer

RUN ln -sf /dev/stdout /var/log/nginx/accsee.log  
只要將需要使用的log轉到 stdout stderr docker就會幫我們處理完，讓我們可以於container外讀取。

CMD ["nginx", "-g", "daemon off;"]  
此CMD為最後要執行的command，每次comtainer啟動或重啟(restart a stop container)都會執行。

FROM:  
image base

WORKDIR:  
get in container path. like cd

COPY:  
COPY host file to current container path


## image tag
tag: 用來標示指向image的標記，多個不同名子的tag可以指向同個image。
`docker image ls` 可以發現不同的tag欄位可能有相同的image ID，表指向相同image(same image for storage saving)。

![alt text](tag.PNG)

give a new tag
```
docker image tag nginx bretfisher/nginx

Usage: docker image tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```
![alt text](tag_newtag.PNG)


push docker (repo)  
如未指定tag，default is latest.  
This login is from docker CLI
```
docker login
docker image push (:repository)[:testing]
```

## docker file
build dockerfile from cmd  
-f : docker file name 
```
docker build -f some-dockefile
```

build custom image, when dockerfile is ready and in here, 
-t: target  
. : current dir  
```
docker image build -t customnginx .
```

prune clean unuse container image
-a : clean all
```
docker image prune
```

## Persisten data 
部分資料為程式產生的獨特資料，需要持續保存(e.g. database)，這些資料不能隨著container關閉而消失。這些資料比起container更重要。  
note: 這些資料要刪除都要手動處理
### volume
將資料夾位置指定為persistent保存空間  
`docker container inspect`` 可以看到Mount中volume的資訊包含在host的位置。

https://github.com/docker-library/mysql/blob/master/8.0/Dockerfile.debian  
![alt text](mysql_volume.PNG)

---

list all volume  
列出 volume name, volume name可以用 `docker volume inspect`查看
```
docker volume ls
```

specify volume name  
-v option: named volume  
container 指定了相同的volume name後，啟動不同container會使用此volume，volume會只有指名volume。(未指明會隨機取碼建立)
```
docker container run ....... -v mysql-db:/var/lib/mysql
```

### Bind Mounting
當`-v`，`:`前方是接host資料夾路徑。  
可以以使用`--mount`。  
資料夾binding (Bind Mounting)使用場景，例如可以加nginx/html資料夾連接local host存在web index html的資料夾，直接展現index page，不須再傳輸index page。

```
docker container run -d --name nginx -p 80:80 -v $(pwd):/usr/share/nginx/html nginx
```


## docker composer
 自動化部屬多個container, env, network...  
 將原本的docker command寫進yaml檔。
```
# version isn't needed as of 2020 for docker compose CLI. 
# All 2.x and 3.x features supported
version: '2'

services:
  drupal:
    image: drupal:9
    ports:
      - "8080:80"
    volumes:
      - drupal-modules:/var/www/html/modules
      - drupal-profiles:/var/www/html/profiles       
      - drupal-sites:/var/www/html/sites      
      - drupal-themes:/var/www/html/themes
  postgres:
    image: postgres:14
    environment:
      - POSTGRES_PASSWORD=mypasswd

volumes:
  drupal-modules:
  drupal-profiles:
  drupal-sites:
  drupal-themes:
```


start and stop
```
docker compose up
docker compose down
```