# Set up github repo and codespaces

After creating a github repo, go to code > create codespace

![alt text](image.png)

> GitHub Codespaces is a cloud-based code editor that runs in your browser. Instead of coding on your local machine, you code in a remote Docker container hosted by GitHub. You can still run Docker inside of this Docker container
> 
> You can check the system it's using with `$ lsb_release -a`
>
> In command palette, you can select open in desktop VS code.

# Basic `$ docker run` options

`$ docker run hello-world`: testing image to make sure docker is working

## -it

`$ docker run ubuntu`: if you run like this it will exit immediately because there's no **interactive terminal** attached. To make sure it works interactively, run `$ docker run -it ubuntu`, where

- `-i` stands for interactive
- `-t` stands for terminal

`$ docker run -it python:3.13.11`: install and run interactively this specific python image version. 

> Without the content after ":", the version will default to "latest". You can check all installed images using `$ docker image ls` 

## --entrypoint

Some images by default open bash and other open python terminal. This is defined by **CMD and/or ENTRYPOINT** in the image’s Dockerfile. You can view it with `$ docker image inspect <image>` (look at Config.Entrypoint and Config.Cmd).
For example, the Dockerfile for python has:

``` json
"Config": {
   "Cmd": [
       "python3"
   ],
   "Entrypoint": null,
```
This means after running the OS for this image it runs python3 immediately, causing us to see the python interface

This can be **overwritten with `--entrypoint` parameter**, like: `$ docker run -it --entrypoint=bash python:3.13.11-slim`

## -v

To allow docker to see files in host machine, use "volumn", which is set up with `-v`.

``` bash
docker run -it \
    --rm \
    -v $(pwd)/test:/app/test \ # this line maps the test folder in host to the app/test folder in container
    --entrypoint=bash \
    python:3.9.16-slim
```

Note that the paths in mapping need to be the **absolute path**, so this works: `-v $(pwd)/test:/app/test`, but this doesnt: `-v test:/app/test`.


# Basic docker commands

- `$ docker run`: see above
- `$ docker ps`: list live containers
  - `$ docker ps -a`: list all containers
  - `$ docker ps -q`: list live containers, only show IDs 
  - `$ docker ps -aq`: list all containers, only show IDs 
- `$ docker rm <container-id>`: remove a stopped container from `$ docker ps -a`
  - `$ docker rm $(docker ps -aq)`: remove a stopped container from `$ docker ps -a`. This format `$(<command>)` replaces the param with the output of the command.

# Basic linux commands

- `$ mkdir test`: create a dir called test
- `$ touch file1.txt file2.txt file3.txt`: create empty files
- `$ echo "hello from host" > file1.txt`: echo prints text to the terminal or a file
- `$ cat file1.txt`: prints file contents to the terminal.
