# HW4-Docker

### Docker Compose

You are ready to launch your cat photo startup company. Use docker compose to setup your HW3 app in the following way:

* Setup a container for redis.
* Setup a container for proxy.
* Setup a container for node app.
* Modify infrastructure.js to spawn new containers instead of new servers.

![Screencast](https://github.com/shivamgulati1991/HW4-Docker/blob/master/Screens/1.gif)

### Docker Deploy 

Extend the deployment workshop to run a docker deployment process.

* On post-receive will build a new docker image.
* Push to local registery.
* Deploy the dockerized [simple node.js App](https://github.com/CSC-DevOps/App). Add appropriate hook commands to pull from registery, stop, and restart containers.

![Screencast](https://github.com/shivamgulati1991/HW4-Docker/blob/master/Screens/2.gif)

### File IO

You want to create a container for a legacy application. You succeed, but you need access to a file that the legacy app creates.

* Firstly, build a new docker image
```
docker build -t <image_name> .
```

* Use socat to map file access to read file container and expose over port 9001 
Lets create a file 'fileiotest.txt' and write some sample content.
```
sudo docker run -td --name server fileio
sudo docker exec -td server sh -c "echo 'This is test file. Hey'>/fileiotest.txt"
sudo docker exec -it server bash

```

Now once you are inside the container, type in the below command:
```
socat tcp-l:9001,reuseaddr,fork system:'cat /fileiotest.txt',nofork
```

* Use a linked container that access that file over network. The linked container can just use a command such as curl to access data from other container.

Open another terminal, and to link a container use the below commands:
```
sudo docker run -td --name client -h client --link server:server fileio
sudo docker exec -it client bash
```

Now you would be inside the container, lets call the curl command to see the text we wrote earlier:
```
curl server:9001
```

You should be able to see the file contents in the terminal now.

![Screencast](https://github.com/shivamgulati1991/HW4-Docker/blob/master/FileIO/3.gif)
