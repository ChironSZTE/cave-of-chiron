# Container Images

Container images are immutable, snapshotted states of a container.
They contain the filesystem of containers (including system files, configuration files, and application files), but also parameters required for properly running a container that is instantiated from it, like the entry point (the program to be started), the user account of the started program, etc.

## Basic Usage

### Obtaining a container image

Although Docker automatically downloads the right container image when a container is run for the first time, you may want to download it earlier than that.  
You can use  `docker image pull <name>` for this purpose:
```
$ sudo docker pull gitea/gitea
Using default tag: latest
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
docker.io/gitea/gitea:latest
```

### Listing downloaded images

Container images can take up quite some storage space.  
To find out which ones you have downloaded, and how much space they take up, you can use the `docker image ls` command:
```
$ sudo docker image ls
REPOSITORY            TAG       IMAGE ID       CREATED        SIZE
gitea/gitea           latest    9d4928991c26   3 weeks ago    257MB
openhab/openhab       3.4.2     51901f4fc8ab   4 weeks ago    724MB
debian                latest    54e726b437fb   5 weeks ago    124MB
linuxserver/mariadb   alpine    dc4c3424fd43   5 months ago   289MB
containous/whoami     latest    0f6fbbedd377   3 years ago    7.37MB
```

### Removing images

If you don't need an image anymore, you can remove it with `docker image rm <name>`:
```
$ sudo docker image rm gitea/gitea
Untagged: gitea/gitea:latest
Untagged: gitea/gitea@sha256:4c54a18fc46fe81b6f1400e3b03f187952de50ff464f199bba1215b71881475f
Deleted: sha256:9d4928991c2691eec88c96573da866cdbb5a6b1c4f4ef6ef2ad1ec52375aab23
Deleted: sha256:4ef365e73bc587daa4164a00a5717708d767ede2b2500fa371b1dc9ff691758e
[...]
```

## Advanced Usage

Management of images are done with subcommands of the `docker image` command.

### Identification

Images are identified by their names and tags, or their "digests": `NAME[:TAG|@DIGEST]`.  
Images can always be referred to in this format.

Their name is a human readable name, usually the name of a project. They might contain different sections that are divided by `/` characters:  
- When obtained from a container registry, the name will also include the user or organization that publishes it (e.g. `gitea/gitea`).  
- Names might also contain the container registry from where they were downloaded (e.g. `lscr.io/linuxserver/wireguard`).

Tags are used to differentiate between different flavors of the image, based on version number, release type (release, development, nightly), base linux distribution, or others.  
The `latest` tag frequently points to the last uploaded container image, which might be a nightly release when you just wanted the latest stable one, so its use is generally discouraged.  
A specific NAME:TAG pair might still have further image variants when different images are built for different "platforms" (CPU architectures).

The digest is the checksum (usually SHA256) of (the top layer of) a container image.

### Additional ways for obtaining images

#### Manually loading images in the OCI image format

Sometimes the image you want to use was built on a different system, and you may not want to upload it to a container registry.  
In that case, you can export and import it in a tar file:
```
docker image save --output gitea_1.18.5.tar local/gitea:1.18.5
docker image load --input gitea_1.18.5.tar
```

Important: do not use the `docker export` and `docker import` commands for this task! They only manage the filesystem of the image, they ignore any metadata stored in it, such as the file to be ran on startup.

#### Uploading to a container registry

First you have to log in to your user account on the container registry to which you want to upload the image, with the `docker login --username <username> --password-stdin [container registry server URL]` command.
Then you can use `docker image push <image>` to start the upload process.

### Creating images

You can also create container images, and for that you have 2 options:
- with the build process using Dockerfiles (the proper way)
- by snapshotting a container, thus preserving its current state in a new immutable layer
