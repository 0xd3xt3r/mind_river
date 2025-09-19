# Docker Daily

Daily commands which I frequently use for docker.

1. **Building Image** 
	`docker build -t osdev .`
	`-t` -> name of the image
1. **Running the image**'
	1. `docker run -i -t ubuntu:20.04 /bin/bash`
		1. `-i` -> interactive
		2. `-t \<tag>` - image you want to run
		3. `\<program>` you want to execute
2. **Pull the image**

```bash

# get list of running docker
docker ps -a

docker image ls
# delete docker by Image
docker rmi <IMAGE ID>

docker start tpmlab
```