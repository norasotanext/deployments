## Fix docker permission

My first solutions was:

usermod -aG docker jenkins
usermod -aG root jenkins
chmod 664 /var/run/docker.sock
But none of them work for me, I tried:

chmod 777 /var/run/docker.sock

- https://stackoverflow.com/questions/69039604/sudo-docker-not-found-while-running-the-jenkins-pipeline
- https://stackoverflow.com/questions/42164653/docker-in-docker-permissions-error/42168499#42168499

## Build

```
docker build -t jenkins .
```

## Run

```
docker run \
	--name jenkins \
	--restart=always \
	--detach \
	--network jenkins \
	--publish 8080:8080 \
	--publish 50000:50000 \
	--volume jenkins-data:/var/jenkins_home \
	--volume jenkins-docker-certs:/certs/client:ro \
	-v $(which docker):/usr/bin/docker \
	-v /var/run/docker.sock:/var/run/docker.sock \
	jenkins
```
