[Login to given app server then switch to root]


Downloading docker compose from git using below link
```
curl -L "https://github.com/docker/compose/releases/download/1.28.6/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

Setting executable permission
```
ll /usr/local/bin/docker-compose
```
```
chmod +x /usr/local/bin/docker-compose
```
```
ll /usr/local/bin/docker-compose
```
Validate by running version
```
docker-compose --version
```

To install docker-ce  need to configure repo from below link
```
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

Installing docker
```
yum install docker-ce docker-ce-cli containerd.io -y
```

Verifying  docker is installed successfully
```
rpm -qa |grep docker
```

Enable & start the docker service
```
systemctl enable docker
```
```
systemctl start docker
```
```
systemctl status docker
```

Validating the task
```
docker --version
```
```
docker ps
```
```
docker-compose --version
```
