# docker-containers
A repo for docker demos and intructions

# How to install docker 
(instrucitons from https://docs.docker.com/engine/install/debian/)

```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

This is an apt source for debian and will contain `bookwork` docker release so no need to change to Ubuntu' `jammy` as I did before.

Now we can install the Docker packages with aptitude:
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

In `~/.docker/config.json` change `credsStore` to `credStore` according to [this](https://forums.docker.com/t/docker-credential-desktop-exe-executable-file-not-found-in-path-using-wsl2/100225/3) discussion. Otherwise `“docker-credential-desktop”: executable file not found in $PATH, out:` error will arise (Might be related to me not being logged in).

## Install docker-desktop [Optional]

Download the latest version of the [Deb package](https://desktop.docker.com/linux/main/amd64/135262/docker-desktop-4.27.0-amd64.deb?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-linux-amd64)

`sudo apt install ./docker-desktop-*.deb`

If you've created images with docker engine before installing the docker-desktop, those won't be visible because of VM specifics. Also, after the installation of the docker-desktop, the docker context will be switched to the newly created `desktop-linux`. Run `docker context list` and `docker context use [blah]` to switch between the contexts.

# How to build and run the most basic contaner

1. Build an image with `docker build --tag hello-internet .`
Here is an output:
```
[+] Building 0.9s (7/7) FINISHED                                                                                                                                   docker:default
 => [internal] load build definition from Dockerfile                                                                                                                         0.0s
 => => transferring dockerfile: 98B                                                                                                                                          0.0s
 => [internal] load metadata for docker.io/library/nginx:1.10.1-alpine                                                                                                       0.9s
 => [internal] load .dockerignore                                                                                                                                            0.0s
 => => transferring context: 2B                                                                                                                                              0.0s
 => [internal] load build context                                                                                                                                            0.0s
 => => transferring context: 129B                                                                                                                                            0.0s
 => [1/2] FROM docker.io/library/nginx:1.10.1-alpine@sha256:dabd1d182f12e2a7d372338dfd0cde303ef042a6ba01cc829ef464982f9c9e2c                                                 0.0s
 => CACHED [2/2] COPY src/html /usr/share/nginx/html                                                                                                                         0.0s
 => exporting to image                                                                                                                                                       0.0s
 => => exporting layers                                                                                                                                                      0.0s
 => => writing image sha256:e0c9151e776ddde77928710a0f9c7f6017885b06f50fdccdee841a315b1af9fd                                                                                 0.0s
 => => naming to docker.io/library/hello-internet                                                                                                                            0.0s

View build details: docker-desktop://dashboard/build/default/default/syxrx9jajsqjznlxcyfmfti3v
```
2. We can now check that the image has appeared in the images list with `docker images`:
```
REPOSITORY       TAG       IMAGE ID       CREATED         SIZE
hello-internet   latest    e0c9151e776d   3 minutes ago   54.7MB
```
3. Now we can run/create a contaner that will listen on localhost port 1024 `docker run -it hello-internet -p 1024:80 e0c9151e776d`
Note the image ID at the end
We get a hash-like response: `f35ce54e0240c65cc6f6dfd460a389392fc852b3d41d3e34521a8e6f820b1f9e`
Now the container is facilitating the connection on localhost:1024
