# Install docker on ubuntu
- `sudo apt-get update`
- `sudo apt install             
 ca-certificates    
 curl    
 gnupg    
 lsb-release`
- `curl -fsSL https://get.docker.com -o get-docker.sh`
- `sh ./get-docker.sh`

# Manage Docker as a non-root user

1. Create the docker group with `sudo groupadd docker`
2. Add your user to the docker group with `sudo usermod -aG docker $USER` (-a --append, -G --groups)
3. Activate the changes with `newgrp docker`
