# jenkins_sample

## setup
1. Build the Jenkins BlueOcean Docker Image

```sh
docker build -t myjenkins-blueocean:2.414.2 .
```

Blue Ocean is a new user experience for Jenkins based on a personalizable, modern design that allows users to graphically create, visualize and diagnose Continuous Delivery (CD) Pipelines

2. Create network 
```
docker network create jenkins
```
here, jenkins is n/w name

3. Running Container

```
docker run --name jenkins-blueocean --restart=on-failure --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins-blueocean:2.414.2
  ```

4. now `docker ps` will give the port its listening in, (as mentioned, 8080). go to localhost:8080. password is in **/var/jenkins_home/secrets/initialAdminPassword** of Container, so, 
`docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword` does the job.


dashboard->new item->freestyle object(or)Pipeline.

freestyle object:[freestyle_object.md](https://github.com/Saru2003/jenkins_sample/blob/main/freestyle_object.md)

pipeline: [pipeline.md](https://github.com/Saru2003/jenkins_sample/blob/main/pipeline.md)
