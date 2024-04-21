# docker_lab2
lab2
# ITI - Docker Lab Two🐋
## Task 1: Run a container using nginx image, and mount a directory from your host into the Docker container.
example: /home/samy/nginx:/home/nginx (bind mount)

### Steps
#### 1. Create Bind Mount Directory
```bash
mkdir file_dir

```

#### 2. Run a container using nginx image
```bash
docker run -d --name file_dir -v /root/file_dir:/user/share/nginx/html nginx
docker exec -it file_dir bash
```

#### 3. Echo any content to show when curl ip-address
```bash
cd /user/share/nginx/html
echo "Hello i'm sara magdi" > index.html
exit
docker inspect -f '{{.NetworkSettings.IPAddress}}' file_dir ->172.17.0.7
curl 172.17.0.7
```

---
## Task 2: Create 2 docker network (net-1 & net-2)

### Steps
#### 1. Create 2 docker network (net-1 & net-2)
```bash
docker network create firstnetwork
docker network create secondnetwork
```

#### 2. Run 2 new containers using nginx:alpine image, and attach the net-1 to them.
```bash
docker run -d --name nginx_firstnetwork --net firstnetwork nginx:alpine
docker run -d --name nginx_secondnetwork --net secondnetwork nginx:alpine
```

#### 3. Run another 1 new containers using nginx:alpine image, and attach the net-2 to them.
```bash
docker run -d --name nginx_thirdnetwork --net secondnetwork nginx:alpine
```

#### 4. Inspect the 3 containers to know their IPs and write them aside.
```bash
docker inspect -f '{{.NetworkSettings.Networks.network_1.IPAddress}}' nginx_firstnetwork
172.18.0.2

docker inspect -f '{{.NetworkSettings.Networks.network_1.IPAddress}}' nginx_secondnetwork
172.18.0.3

docker inspect -f '{{.NetworkSettings.Networks.network_2.IPAddress}}' nginx_thirdnetwork
172.20.0.2
 docker inspect nginx_firstnetwork | grep IPAddress
```

#### 5. Enter a container in the net-1 network and try to ping a container in the net-2 network (What do you notice?)
```bash
docker exec -it nginx_firstnetwork sh 
Use ping or curl
ping 172.20.0.2
ping fails cuz of same network
```

#### 6. Enter a container in the net-1 network and try to ping the other container in the same network (What do you notice?)
```bash
docker exec -it nginx_firstnetwork sh 
curl 172.18.0.2
ping 172.18.0.3
ping successful cuz both are in same network
```

## Task 3: Explain the difference between Docker volumes and Bind Mount.
dockervolumes :
1-Managed by Docker.
2-Separate, organized storage spaces.
3-Can be shared among containers
4-Docker keeps track of them
Bind Mounts:

Bind Mounts:
1-Direct connections between host and container.
2-Host folders directly accessible inside container.
3-Changes reflect instantly in both places.
4-Great for development/testing files.