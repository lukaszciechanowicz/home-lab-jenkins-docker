# home-lab-jenkins-docker

My home Lab dockerised Jenkins with data that persists.
Just leaving notes here...

# Preparing the data volume

Preparing my data volume.

```
docker volume create jenkins-data
docker volume ls | grep jenkins-data
```


As a super pro tip, if docker subsystem is eating a lot of disk space, it might be because of lot of “zombie” volumes lying around. 
These are data volumes that no longer have owners/containers. They can be pruned with:

```
docker volume prune
```

# Preparing the log volume

```
docker volume create jenkins-log
```

# Building and running my Home Lab Jenkins
```
docker stop jenkins-master
docker rm jenkins-master
docker build -t myjenkins .
docker run -p 8080:8080 -p 50000:50000 --name=jenkins-master --mount source=jenkins-log,target=/var/log/jenkins --mount source=jenkins-data,target=/var/jenkins_home -d myjenkins

docker exec jenkins-master cat /var/jenkins_home/secrets/initialAdminPassword

```

Docker is smart enough to apply the read/write permissions of the target directories that have been assigned in Dockerfile when it mounts in the volume. 

```
docker exec jenkins-master tail -f /var/log/jenkins/jenkins.log
```