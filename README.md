# My frequent Docker commands

## 📜 **list** containers/images
* `docker ps -a` <--- show even stopped **containers**
* `docker images -a` <--- show even unused **images**
## ✅ **build/run** an image
* `docker build -t abc .` <--- build a container in a current directory
    * `docker build -f docker/base/Dockerfile -t abc .` <--- build a Docker image from another path
    * `docker build -f docker/def/Dockerfile --build-arg BASE_IMAGE=abc -t def` <--- pass a build argument
* `docker run -it abc bash` <--- run Docker image in an interactive bash shell
    * `docker run --name xyz -it abc bash` <--- give it a name
* `docker exec -it xyz bash` <--- get into a running container (first make sure to `docker start xyz` if stopped)
## 💾 **mount** volume to a container ([docs](https://docs.docker.com/storage/volumes/#start-a-container-with-a-volume))
* `docker run --name xyz -v $pwd:/app -it abc bash` <--- mount current directory to a container and run it
    * you'll be able able to access your local folder in the /app folder of the Docker image
    * ⚠ if you're using a terminal on Windows, make sure to replace `$pwd` (see more info in a [section below](https://github.com/pyxelr/my-frequent-docker-commands/edit/main/README.md#-running-docker-command-on-windows))
## 🌐 **publish** a container (e.g. to use REST/curl)
* `docker run -it --rm -p 8080:80 abc` <--- expose port 8080 (inside container) to port 80 (on the host), and automatically remove the container after exiting 
* `docker run -d -p 8080:80 abc` <--- run in a detached mode (detach from the container and return to the terminal prompt)
## 🚶‍♂ **exit** a container
* `[CTRL] + [D]` or `exit` <--- exit and stop the container
* `[CTRL] + [P] + [Q]` <--- exit without stopping the container
## ❌ delete containers/images
* `docker system prune -a --volumes` <--- remove **all** (stopped containers, unused images, volumes)
* `docker stop $(docker ps -a -q)` <--- stop all running **containers**
    * `docker stop <container_id>` <--- stop a running container
* `docker rm $(docker ps -a -q)` <--- remove all stopped **containers**
    * `docker rm -f <id/name>` <--- remove a container forcefully
* `docker rmi $(docker images -q)` <--- remove all **images**
    * `docker rmi -f <image_id>` <--- remove image by its ID forcefully
    * `docker image rm -f <name>` <--- remove image by its name
* `docker volume rm $(docker volume ls -q)` <--- remove all **volumes**
---
## ⚠ running Docker commands on Windows
* **cmd.exe**
    * replace `$pwd` with `%cd%`
    * replace any newline characters `\` with `^`. If you are using `bash -c "…"`, then add `^` and make a newline before the first `"`
* **PowerShell**
    * replace `$pwd` with `${pwd}`
    * replace any newline characters `\` with `` ` `` (but not the ones within `bash -c "…"`)
* **Git Bash**
    * replace `$pwd` with `/$(pwd)`
    * you may need to precede the command with `winpty` (at least not in the VS Code terminal)
    * it is safer to use `bash` instead of `/bin/sh`
    * if you're specifying a working directory, instead of `-w /app` use `-w "//app"`
---
You can find more commands in the official [Docker CLI Reference](https://docs.docker.com/engine/reference/run/).
