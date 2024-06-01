## in freestyle object:
in source code mgmt, use git if using a github link, if private, use appropriate PAT with appropriate permissions, or use a public repo without the need of credentials. if any triggers are required, use poll scm (just like cronjob, `*/5 * * * *` means every 5 mins). 
to build env afresh, delete workspace before build starts can be used. in post build actions->execute shell. do whatever linux commands you need. test with some echo commands and save and build.

create some files, which are stored in /var/jenkins_home/workspace, where the job created is as folder, execute:
`docker exec -it jenkins-blueocean sh` 

## setting up docker agent
in new node->new node->in plugins->install docker plugins->refresh.

then add docker as new cloud. 

**alpine/socat container to forward traffic from Jenkins 
to Docker Desktop on Host Machine**: 
`docker run -d --restart=always -p 127.0.0.1:2376:2375 --network jenkins -v /var/run/docker.sock:/var/run/docker.sock alpine/socat tcp-listen:2375,fork,reuseaddr unix-connect:/var/run/docker.sock`

`docker inspect <container_id> | grep IPAddress`, use that ipaddr in docker host uri:tcp://<ip>:2375

then add new docker agent template, use docker image like: jenkins/agent:alpine-jdk11, remote file sys root is `/home/jenkins`.

use this template, in the job created, in *restrict where this project can be run*. say, we are running a python script, and dependancy arent met. so, we can create our own image, (https://hub.docker.com/repository/docker/sarvesh20123/jenkinsagent/general), create a new docker agent template, with the image just created, replace the label expression.
