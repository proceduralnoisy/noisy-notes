# Docker FAQ

## Basic Usage

### Obtaining an image (docker.desktop)
Use the search bar at the top to find an image of interest.

Example (ubuntu):
* Search for 'ubuntu' and select it
* Select a 'Tag' at the right to choose a version (e.g. '24.04')
* Click 'Pull'. The image will now appear in the 'Images' tab

### Starting a new container with an interactive shell
* Determine the image name (`docker image ls`)
* Run `docker run -it [--name NAME] IMAGE_NAME:TAG bash`
  * Example: `docker run -it --name my-ubuntu ubuntu:24.04 bash`

The container will now appear in the 'Containers' list. 

### Listing containers (docker.desktop)
* Show all containers: `docker ps --all` (or `docker container list --all`)
* Show running containers: `docker ps`