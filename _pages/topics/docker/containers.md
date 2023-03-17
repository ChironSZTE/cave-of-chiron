# Docker Containers

## Basic Usage

## Advanced usage
<további részletek, you can read more here: dokum,hi8vatalos dokumentáció>

## Example

Below is an example run of the command.
It can be seen that Docker first downloaded the image layer-by-layer, then started the web service inside it after "Status: Downloaded newer image for gitea/gitea:latest".
```
$ sudo docker run gitea/gitea
Unable to find image 'gitea/gitea:latest' locally
latest: Pulling from gitea/gitea
ef5531b6e74e: Pull complete 
ab8bb6fd5d53: Pull complete 
e24dd90f528a: Pull complete 
1c9e4370e57b: Pull complete 
4b30b17675f2: Pull complete 
9b32fd3f7863: Pull complete 
bdf38634abf8: Pull complete 
83d2e7692aa6: Pull complete 
Digest: sha256:4c54a18fc46fe81b6f1400e3b03f187952de50ff464f199bba1215b71881475f
Status: Downloaded newer image for gitea/gitea:latest
Generating /data/ssh/ssh_host_ed25519_key...
Generating /data/ssh/ssh_host_rsa_key...
Generating /data/ssh/ssh_host_dsa_key...
Generating /data/ssh/ssh_host_ecdsa_key...
Server listening on :: port 22.
Server listening on 0.0.0.0 port 22.
2023/03/17 17:22:53 cmd/web.go:106:runWeb() [I] Starting Gitea on PID: 17
2023/03/17 17:22:53 ...s/install/setting.go:21:PreloadSettings() [I] AppPath: /usr/local/bin/gitea
2023/03/17 17:22:53 ...s/install/setting.go:22:PreloadSettings() [I] AppWorkPath: /app/gitea
2023/03/17 17:22:53 ...s/install/setting.go:23:PreloadSettings() [I] Custom path: /data/gitea
2023/03/17 17:22:53 ...s/install/setting.go:24:PreloadSettings() [I] Log path: /data/gitea/log
2023/03/17 17:22:53 ...s/install/setting.go:25:PreloadSettings() [I] Configuration file: /data/gitea/conf/app.ini
2023/03/17 17:22:53 ...s/install/setting.go:26:PreloadSettings() [I] Prepare to run install page
2023/03/17 17:22:53 ...s/install/setting.go:29:PreloadSettings() [I] SQLite3 is supported
2023/03/17 17:22:54 cmd/web.go:220:listen() [I] [6414a1ee] Listen: http://0.0.0.0:3000
2023/03/17 17:22:54 cmd/web.go:224:listen() [I] [6414a1ee] AppURL(ROOT_URL): http://localhost:3000/
2023/03/17 17:22:54 ...s/graceful/server.go:62:NewServer() [I] [6414a1ee] Starting new Web server: tcp:0.0.0.0:3000 on PID: 17
```

---

#### Creation

When you want to create a new container, you do it by executing the `docker [options] run <image>` command.
Options are optional, the image specification is mandatory.
I'll explain "images" and their identification soon.

If you ran an image for the first time, Docker will first download it automatically for you (by default from the [Docker Hub](https://hub.docker.com) container registry).  
When the download has finished, or if the image was already downloaded, the container will be started, and the software running in it will do its work, whether it is a one-off script (a utility for e.g. file conversion), or a network service (e.g. a website).

The container runs until the main process inside it exits, or you stop it by pressing `Ctrl+C`. When that happens, you will see the exit code printed in the terminal.

The above command also accepts options that modify the behavior of it.
Some of the more useful are the following.
You needn't memorize them right away, you can always come back when you remember one of them might be useful for you.

|Option|Meaning|  
|---|---|
|-d, --detach|Run in background, without showing container logs|
|--name|Assign a name for easier identification. Otherwise the container will get a randomly generated human readable name|
|--rm|Delete the container on exit. Useful for temproary containers|
|--network|Attach the container to a preexisting Docker network by its name|
|-p, --publish|Bind networks ports of container to Host's network interfaces (all of them), so that they are accessible from the networks where the Host has an interface|
|--volume|Attach the specified Docker storage volumes to this container|
|-e \<envvar list>, --env \<envvar list>|Run with the given environment variables|
|-i, --interactive|Keep STDIN stream open, so that the software running in the container can be controlled through it. Frequently used with `-t` for making it accessible from a terminal|
|-t, --tty|Allocate a virtual terminal. Frequently used with `-i`|
|--help|Lists all available options, including those that were not listed here|



#### Stopping and restarting

#### Deletion