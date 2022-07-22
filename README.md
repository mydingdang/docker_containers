# Deploy Docker containers for Jupyter Notebook
## Deploy docker on ubuntu
### Download and untar package
- `sudo apt-get update`
- `sudo apt install -y             
 ca-certificates    
 curl    
 gnupg    
 lsb-release`
- `curl -fsSL https://get.docker.com -o get-docker.sh`
- `sh ./get-docker.sh`

### Manage Docker as a non-root user

1. Create the docker group with `sudo groupadd docker` (It may shows that the docker group already exists, skip it.)
2. Add your user to the docker group with `sudo usermod -aG docker $USER` (-a --append, -G --groups)
3. Activate the changes with `newgrp docker`

## Deploy Jupyter Notebook on Docker containers

1. Download Ubuntu:20.04 image on Docker as base image using`docker pull ubuntu:20.04`
2. Build the "ic3jupyter" image based on the dockerfile-jupyter, using the command`docker build -f dockerfile-jupyter -t ic3jupyter:2.00 .`. In dockerfile-jupyter file we use the pulled ubuntu:20.04 image with `FROM ubuntu:20.04`,update apt-get tools and install jupyter-notebook normally. After the image built, we can check the dockerfile of the image with`docker history --no-trunc ic3jupyter:2.00`.
3. Vital steps to realize the self-starting function of Jupyter-notebook are to write a jupyter_notebook_config.py file and to `ADD` it to the `/usr/local/config/`directory in the container image.
4. We can create a container based on ic3jupyter image using `docker run -it -p 9090:9090 --name=notebook ic3jupyter:2.00`.Then it will be like this.
![avatar](https://github.com/mydingdang/docker_containers/blob/main/create.png?raw=true)
5. We can open the working page on the browser of Ubuntu Machine at http://localhost:9090/ as the ip is already mapped to the localhost. **If we connect the Ubuntu server wich RPC, in order to open the web page of the Jupyter-notebook on the container on the remote server, we use `ssh -L 9090:localhost:9090 <host@ip>`** .With this command the port on the Jupyter server is mapped **twice** to our real localhost:9090. 


### In the config file:
- We need to set up the `c.NotebookApp.allow_root = True` to allow notebook opened by root user.
- We need to set the `c.NotebookApp.ip=ip` where ip means the ip adress of the Ubuntu server instead of `localhost`, we can realize that with the socket package in Python easily.
- We set the port to be 9090 and notice that we need to map this port to the machine port.
- We need to set`c.NotebookApp.notebook_dir = '/usr/local/jupyter'` as the specific work space(Remember to seperate it with `/usr/local/config/`).
- We need to set `c.NotebookApp.password =''`, and here the password string must be SHA1 hased form. We can switch it with `from notebook.auth import passwd; passwd()`.
