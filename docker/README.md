# Docker
## Docker desktop
You can use docker desktop to implement docker or even a kubernetes cluster on your machines
| HOST       | DNS                        | PORT |
| ---------- | -------------------------- | ---- |
| kubernetes | kubernetes.docker.internal | 6443 |
| Docker     | host.docker.intenral       | 2375 |

## WSL
In order to install docker with wsl follow these commands:
```sh
wsl --set-default-version 2
# Get list of available online distributions
wsl -l -o 
# install a distribution
wsl --install -d Ubuntu-18.04

# Get info about installed distributions
wsl --list --verbose
# Change the wsl version of distribution
wsl --set-version Ubuntu-18.04 2
# Select one of the distributions
wsl -d Ubuntu-18.04
# This will shut them all down
wsl --shutdown
# To delete a distribution
wsl --unregister Ubuntu-18.04
```
If it's requiring an update you can run `wsl --update` or visit this [link](https://www.catalog.update.microsoft.com/Search.aspx?q=wsl) download the msi install it then run `wsl`
## Hyper-V
### Issue with WSL2
Make sure Hyper-V is enabled, (works only on windows-pro) by going to **Turn windows features on or off** -> check **Hyper-V** box

If you chose to enable WSL in the installation you can disable it by updating 
```json
// C:\Users\<username>\AppData\Roaming\Docker\settings.json
{
  "wslEngineEnabled": false // false instead of true
}
```

# Add insecure registry
go to `%USERPROFILE%/.docker/daemon.json` then add this line
```json
{
  "insecure-registries": ["regsitryip:port"],
  //...
}
```
# Additional Info & Documentation
1. Run docker command and mount socket (Docker in Docker)
```sh
# Mount docker host
docker run -d --name dind -v /var/run/docker.sock:/var/run/docker.sock -v /usr/bin/docker:/usr/bin/docker -v ${pwd}:/app -w /app ubuntu bash -c "tail -f /dev/null" 
# Get into the docker container
docker exec -it dind bash
# Mount docker without docker bin
docker run -d --name dind -v /var/run/docker.sock:/var/run/docker.sock -v ${pwd}:/app -w /app ubuntu bash -c "tail -f /dev/null" 
docker exec -it dind bash
apt-get update && apt-get install curl -y && curl -fsSL https://get.docker.com | sudo sh
```
1. Run docker command and mount socket
```sh
# Docer on Docker
docker run --privileged -d --name docker -v ${pwd}:/app -w /app docker:dind
# Get into the docker container
docker exec -it docker sh
```

# host.docker.internal on linux
On Linux, add `--add-host=host.docker.internal:host-gateway` to your Docker command to enable this feature. 
Docker Compose on Linux:
```yaml
extra_hosts:
    - "host.docker.internal:host-gateway"
```