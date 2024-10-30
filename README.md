# Pasos para correr jenkins con docker

1. Descargar la imgane de docker-jenkins y crea contenedor

```
docker run --name jenkins-docker --rm --detach --privileged --network jenkins --network-alias docker --env DOCKER_TLS_CERTDIR=/certs --volume jenkins-docker-certs:/certs/client --volume jenkins-data:/var/jenkins_home --publish 2376:2376 docker:dind --storage-driver overlay2
```

2. Creamos la imagen del Dockerfile

```
docker build -t myjenkins-blueocean .
```

3. Corremos contenedor con imagen antes creada

```
docker run --name jenkins-blueocean --restart=on-failure --detach --network jenkins --env DOCKER_HOST=tcp://docker:2376 --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 --publish 8088:8080 --publish 8089:50000 --volume jenkins-data:/var/jenkins_home --volume jenkins-docker-certs:/certs/client:ro myjenkins-blueocean
```

4. Obtenemos el password de seguridad
```
docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword
```

5. Navegamos a localhost:8088 y pegamos el password antes obtenido

6. Si queremos interactuar con el contenedor
```
docker exec -ti jenkins-blueocean bash
```