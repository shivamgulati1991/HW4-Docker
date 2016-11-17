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
The post-recieve hook has been added to green image. All the commands are together in hook file.
Similar commands can be added for blue image. On seeing changes in repository and when user pushes as below
 ```
 git push green master
 ```

* On post-receive will build a new docker image.

```
sudo docker build -t "newgreenimage" /home/shivam/Deployment/deploy/green-www/
sudo docker tag newgreenimage localhost:5000/newgreenimage:latest
```

* Push to local registery.
```
sudo docker push localhost:5000/newgreenimage:latest
```

* Deploy the dockerized [simple node.js App](https://github.com/CSC-DevOps/App). Add appropriate hook commands to pull from registery, stop, and restart containers.
```
sudo docker pull localhost:5000/newgreenimage:latest
sudo docker stop green
sudo docker rm green

#run the new image at 3005
sudo docker run -td --name green -p 3005:8080 newgreenimage sh -c "node /src/main.js 8080"
```

![Screencast](https://github.com/shivamgulati1991/HW4-Docker/blob/master/Deploy/2.gif)

### File IO

You want to create a container for a legacy application. You succeed, but you need access to a file that the legacy app creates.
'fileio' is our image name.

* Firstly, build a new docker image
```
docker build -t fileio .
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
