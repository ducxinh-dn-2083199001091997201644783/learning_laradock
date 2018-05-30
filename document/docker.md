0.
```
Down
    docker-compose down
Remove mysql/data
    rm -rf ~/.laradock
Remove Useless Images
    docker rmi [all useless images id]
Clean all useless docker volume
    sudo docker volume rm $(sudo docker volume ls -qf dangling=true)
Buil
    sudo docker-compose up -d --build nginx mysql
```

1. Clean
```
docker system prune
docker container prune
docker image prune
docker volume prune
docker network prune
```

2. Build
```
docker-compose up -d --build    mysql
docker-compose build --no-cache mysql
```

1.  Purging All Unused or Dangling Images, Containers, Volumes, and Networks
    - ( clean up any resources:  images, containers, volumes, and networks)
```
docker system prune
```
    - To additionally remove any stopped containers and all unused images (not just dangling images), add the -a flag to the command:
```
docker system prune -a
```

2. Removing Docker Images
```
List: docker images -a
Remove: docker rmi Image Image
```

3. Remove dangling images
```
List: docker images -f dangling=true
Remove: docker images purge
```

4. Remove all images
```
List: 
    docker images -a
Remove:
    docker rmi $(docker images -a -q)
        2: docker rmi $(docker images -a --filter=dangling=true -q
    docker rmi -f $(docker images -aq)
```

5. Removing Containers
    - Use the docker ps command with the -a flag to locate the name or ID of the containers you want to remove:
```
List: docker ps -a
Remove: docker rm ID_or_Name ID_or_Name
```

6. Remove all exited containers
```
List: docker ps -a -f status=exited
Remove: docker rm $(docker ps -a -f status=exited -q)
```

7. Remove all containers and stop
```
List: docker ps -a
Remove: 
    docker stop $(docker ps -a -q)
    docker rm $(docker ps -a -q)
        2: docker rm $(docker ps --filter=status=exited --filter=status=created -q

```

8. Remove Volume
```
List:
    docker volume ls
Remove:
    docker volume rm volume_name volume_name
```

9. Remove dangling volumes
```
List:
    docker volume ls -f dangling=true
Remove:
    docker volume prune
```
10. Remove all volume
```
docker volume prune
```

10. Remove a container and its volume
```
docker rm -v container_name
```

11. Info
```
docker info
```

12. List container and images
```
docker ps -a && echo '--- \n' && docker images -a
```

13. Log
```
docker-compose logs mysql
```