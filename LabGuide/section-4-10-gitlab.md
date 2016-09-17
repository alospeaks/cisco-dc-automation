#Gitlab

##Docker container
1. Create a new folder called `gitlab` under `training` folder.
2. Switch to terminal window
3. go to gitlab folder
4. create a gitlab container using a prebuild image on dockerhub.

docker run --name gitlab -d \
    --volume /Users/hemakuma/training/gitlab:/home/git/data \
sameersbn/gitlab



```
docker run -it \
    --hostname gitlabv2 \
    --publish 443:443 --publish 80:80 --publish 2222:22 \
    --name gitlabv2 \
    --restart always \
    --volume /Users/hemakuma/training/gitlab/config:/etc/gitlab \
    --volume /Users/hemakuma/training/gitlab/logs:/var/log/gitlab \
    --volume /Users/hemakuma/training/gitlab/logs/reconfigure:/var/log/gitlab/reconfigure \
    --volume /Users/hemakuma/training/gitlab/data:/var/opt/gitlab \
    gitlab/gitlab-ce:latest
```


https://docs.gitlab.com/omnibus/docker/
http://stackoverflow.com/questions/35562995/running-gitlab-docker

#Docker compose

docker-compose.yml
web:
  image: 'gitlab/gitlab-ce:latest'
  restart: always
  hostname: 'gitlab.example.com'
  environment:
    GITLAB_OMNIBUS_CONFIG: |
      external_url 'https://gitlab.example.com'
  ports:
    - '80:80'
    - '443:443'
    - '22:22'
  volumes:
    - '/srv/gitlab/config:/etc/gitlab'
    - '/srv/gitlab/logs:/var/log/gitlab'
    - '/srv/gitlab/data:/var/opt/gitlab'
