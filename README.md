# Deploy Docker containers for Jupyter Notebook
## deploy docker on ubuntu
### download and untar package
- `sudo apt-get update`
- `sudo apt install -y             
 ca-certificates    
 curl    
 gnupg    
 lsb-release`
- `curl -fsSL https://get.docker.com -o get-docker.sh`
- `sh ./get-docker.sh`

### Manage Docker as a non-root user

1. Create the docker group with `sudo groupadd docker` (It may shows that the docker group already exists. Skip it.)
2. Add your user to the docker group with `sudo usermod -aG docker $USER` (-a --append, -G --groups)
3. Activate the changes with `newgrp docker`

## Deploy jupyter Notebook on Docker containers

1. Download Ubuntu:20.04 image on Docker as base image using`docker pull ubuntu:20.04`
2. Build the "ic3jupyter" image based on the dockerfile-jupyter, using the command`docker build -f dockerfile-jupyter -t ic3jupyter:2.00 .`.In `dockerfile-jupyter`file we use the pulled ubuntu:20.04 image with `FROM ubuntu:20.04`,update `apt-get` and install jupyter-notebook normally.
3. Vital steps to realize the self-starting function of Jupyter-notebook are to write a jupyter_notebook_config.py file and to add it to the `/usr/local/config/`directory in the container image.

### In the config file:
- We need to set up the `c.NotebookApp.allow_root = True` to allow notebook opened by root user.
- We need to set the `c.NotebookApp.ip=ip` where ip means the ip adress of the Ubuntu server instead of `localhost`, we can realize that with the socket 

