# appd-python-agent
Simple Docker image containing a startup script to enable monitoring Python applications in Kubernetes with an InitContainer.

## Usage
See example at <https://github.com/konrness/ContainerLabs/tree/master/Labs/Lab-1.4-Python>

## To Build Image
    $ docker login
    $ docker build -t konrness/appdynamics-python-agent .
    $ docker push konrness/appdynamics-python-agent

