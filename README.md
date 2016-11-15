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

```
sudo docker run -td --name server test


```

* Use a linked container that access that file over network. The linked container can just use a command such as curl to access data from other container.

![Screencast](https://github.com/shivamgulati1991/HW4-Docker/blob/master/Screens/3.gif)
