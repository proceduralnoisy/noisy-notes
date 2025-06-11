# Docker FAQ

## Basic Usage

### Obtaining an image (docker.desktop)
Use the search bar at the top to find an image of interest.

Example (ubuntu):
* Search for 'ubuntu' and select it
* Select a 'Tag' at the right to choose a version (e.g. '24.04')
* Click 'Pull'. The image will now appear in the 'Images' tab

### Starting a new container with an interactive shell (docker.desktop)
* Open the Terminal (click 'Terminal' at the bottom-right)
* Determine the image name (`docker image ls`)
* Run `docker run IMAGE_NAME -it bash`