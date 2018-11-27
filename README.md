# Mastering Docker
Some examples I want to keep track of as I work through [Shipping Docker](https://course.shippingdocker.com/)

# Notes

Notes from the Shipping Docker series (WIP - for personal reference, will organize/clean-up
in the future)

---

## Module 1

### Docker Commands

  * docker - the main command, manage the life cycle of docker containers
  * docker-compose - coordinate the use of multiple containers (services) using a yaml file
  * docker-machine - spin up servers, install Docker, and let us control them remotely

#### `docker`
```bash
  # Check for running containers:
  docker ps

  # Check for existing containers, whether running or not
  docker ps -a

  # View local images
  docker images

  # example "first" container
  # Lists out the current working directory
  # inside the container
  docker run ubuntu:16.04 ls -lah

  # Outputs json, an array of containers
  docker inspect container_name_or_id

  # Get main IP address
  docker inspect --format="{{.NetworkSettings.IPAddress}}" container_name_or_id
  # Get IP address of all connected networks
  docker inspect --format="{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}" container_name_or_id

  # with jq (json parse utility) installed (`brew install jq`)
  docker inspect container_name_or_id | jq
  # Parse the output, get main IP Address
  docker inspect container_name_or_id | jq -r ".[0].NetworkSettings.IPAddress"
  # Parse the output, get "bridge" network IP Address (same as above)
  docker inspect container_name_or_id | jq -r ".[0].NetworkSettings.Networks.bridge.IPAddress"

  # Cleaning Up Containers
  # remove a comtainer (permanently)
  docker rm container_name_or_id
  # remove all runing containers (permanently)
  docker rm $(docker ps -a)
  # remove ALL containers (permanently)
  docker rm $(docker ps -aq)
  # remove a container when it's done running
  docker run --rm ubuntu:16.04 ls -lah

  # Cleaning Up Images
  # Remove a specific image by ID or Name
  docker rmi image_id_or_name:tag_combo

  # Remove all images
  docker rmi $(docker images -q)

  # Interacting with a container via bash
  # add -i and it flags to run command
  # -i, --interactive: Keep STDIN open even if not attached. This means we can input commands into the running process by typing
  # -t, --tty: Allocate a pseudo-TTY, letting us get output as if we're logged into the container
  docker run -it ubuntu:16.04 bash

  # Sharing ports
  # use -p flag (-p host machine port:container port)
  docker run -it -p 80:80 ubuntu:16.04 bash

  # Sharing volumes
  # use -v flag (-v host directory:container directory)
  docker run -it -p 80:80 -v ~/dockertest:/var/www/html ubuntu:16.04 bash
```

# Useful Snippets

Collection of snippets and use-cases from the Shipping Docker series that
I think will come in handy (WIP - for personal reference, will organize/clean-up
in the future)

---

### mysqldump

Use new/temp container to connect to existing database in network and dump a
table - writes output to pwd of host machine

```bash
$ docker run -it --rm \
  --network=sd_sd-net \
  mariadb:latest \
  mysqldump -h mysql -u root -proot my-app > my-app.sql
```