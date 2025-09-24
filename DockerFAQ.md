# Docker FAQ

## Basic Usage

### Obtaining an image (command line)
Run `docker pull IMAGE_NAME:TAG`

### Obtaining an image (docker.desktop)
Use the search bar at the top to find an image of interest.

Example (ubuntu):
* Search for 'ubuntu' and select it
* Select a 'Tag' at the right to choose a version (e.g. '24.04')
* Click 'Pull'. The image will now appear in the 'Images' tab

### Starting a new container with an interactive bash shell
* Determine the image name (`docker image ls`)
* Run `docker run -it [--name NAME] IMAGE_NAME:TAG bash`
  * Example: `docker run -it --name my-ubuntu ubuntu:24.04 bash`

The container will now appear in the 'Containers' list. 

### Starting a stopped container with an interactive shell
Assuming the image was created in interactive mode, run `docker start -ia IMAGE_NAME`

> **Note:** When using docker.desktop, running a container via the UI will always start `/bin/sh`, **not** `/bin/bash`. Either start via command line as specified above, or invoke bash manually after starting via the UI. See [here](https://stackoverflow.com/questions/76833073/how-to-start-a-existing-but-stopped-container-with-interactive-terminal)

### Listing containers (docker.desktop)
* Show all containers: `docker ps --all` (or `docker container list --all`)
* Show running containers: `docker ps`