1. Run Host
```
docker-machine start vitual_machine_name
```

2. Create docker-machine
```
docker-machine create -d virtualbox vitual_machine_name
Step:
    - Copying /Users/xinhnguyend./.docker/machine/cache/boot2docker.iso to 
    /Users/xinhnguyend./.docker/machine/machines/default/boot2docker.iso.
    - Creating VirtualBox VM
    - Creating SSH key
    - Starting the VM
    - Check network to re-create if needed
    - Waiting for an IP
    - Copying certs to the local machine directory
    - Copying certs to the remote machine
```

3. Command to show configure your shell:
```
eval $(docker-machine env vitual_machine_name)
```

4. check Ip
```
docker-machine ip vitual_machine_name
```
5. List machine
    - See which machine is “active” (a machine is considered active if the DOCKER_HOST environment variable points to it).
```
docker-machine ls
```

6. COnfig
```
docker-machine config
```

7. Inspect
```
docker-machine inspect dev

```

8. Kill
```
docker-machine ls
docker-machine kill `default`
```

9. mount
    - Mount directories from a machine to your local host, using sshfs
    - The notation is machinename:/path/to/dir for the argument; you can also supply an alternative mount point (default is the same dir path).
```
mkdir foo
docker-machine ssh dev mkdir foo
docker-machine mount dev:/home/docker/foo foo
touch foo/bar
docker-machine ssh dev ls foo
```

10. Restart
```
docker-machine restart `default`
```

11. Remove
```
docker-machine ls
docker-machine rm default 
```

12. scp
```
cat foo.txt
docker-machine ssh dev pwd
docker-machine ssh dev 'echo A file created remotely! >foo.txt'
docker-machine scp dev:/home/docker/foo.txt .
cat foo.txt
```

13. ssh
```
docker-machine ssh machinename

docker-machine ssh dev free
```

14. Status
```
docker-machine status
docker-machine status default
```

15. Stop
```
docker-machine stop `machine name`
```