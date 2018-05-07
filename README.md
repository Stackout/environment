# Sudio.co's Staging, Development and Production Enviornment

Project was designed to host a private gitlab repository inhouse, and developing private satis packages for laravel.

## Getting Started

These instructions will get your docker image working to deploy a gitlab and nginx proxy.

### Prerequisites
Here is a list of the prerequisites to get the environment working
```
Docker
```

### Installing 

Install docker
```bash
$ curl -sSL https://get.docker.com | sh
```

Initilize docker swarm
```bash
$ docker swarm init
```

You should get a command to join workers to the swarm like this:

```bash
$ docker swarm join \
    --token SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c \
    192.168.99.100:2377
```
To amke sure the swarm is initilized:
```bash
$ docker node ls
```
Next, we're going to deploy the stack to the swarm. We'll need to do it one at a time.

First, create the registry
```bash
$ docker service create --name registry --publish published=5000,target=5000 registry:2
```

Check that it's working with curl:
```bash
$ curl http://localhost:5000/v2
```

Test the app with compose first to make sure everything is ok.
```bash
$ docker-compose -f docker-compose.yml -f docker-compose-gitlab.yml up -d
```

Bring the app down
```bash
$ docker-compose down --volumes
```

Push the generated image to the reigstry
```bash
$ docker-compose -f docker-compose.yml -f docker-compose-gitlab.yml push
```

Deploy the stack to the swarm
```bash
$ docker stack deploy --compose-file docker-compose.yml environment
$ docker stack deploy --compose-file docker-compose-gitlab.yml environment
```

Next, if gitlab is expierencing some issues, you'll need to pipe in to the gitlab container and migrate the database.

```
$ sudo gitlab-rake db:migrate
```
Now reconfigure gitlab.
```
$ sudo gitlab-ctl reconfigure
```
Check gitlab status
```
$ sudo gitlab-ctl status
```