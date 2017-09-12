# Linaro's Automated Validation Architecture (LAVA) Slave Docker Container
Preinstalls and preconfigures the latest LAVA dispatcher release.

## Building
To build an image locally, execute the following from the directory you cloned the repo:

```
sudo docker build -t lava-slave .
```

## Running
To run the image from a host terminal / command line execute the following:

```
sudo docker run -it -v /dev:/dev -p 69:69/udp -h <HOSTNAME> --privileged kernelci/lava-slave-docker:latest
```
Where HOSTNAME is the hostname used during the container build process (check the docker build log), as that is the name used for the worker configuration. You can use `lava-docker` as the pre-built container hostname.

## Runtime Enviroment
Enviroment variables are available to help setup state within the container.

Where LAVA_MASTER is the IP address for your LAVA server.

```
sudo docker run -it -v /dev:/dev -p 69:69/udp -e LAVA_MASTER='<lava master ip>' -e LAVA_SERVER_IP='<docker host ip>' -h <HOSTNAME> --privileged kernelci/lava-slave-docker:latest
```

Where LAVA_SERVER is the IP of your Docker host. This allows the TFTP service to properly address the TFTP transfers.

## Additional Setup
In order for TFTP requests to find their way back to the running container, you will need to describe the host IP address to the LAVA master node. You can to create a yaml file on the LAVA master node as described below.

```
echo "dispatcher_ip: <dispatcher host ip" > /etc/lava-server/dispatcher.d/<lava-slave-hostname>.yaml
```
