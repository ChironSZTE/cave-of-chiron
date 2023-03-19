---
title: Docker Volumes
tag: Docker
---

Volumes are used for persistent file storage.  
They allow you to store contents of directories outside of containers, and in turn independently of their state.
This means that when you delete and recreate them (e.g. because you have updated them), files stored in volumes are kept.

Volumes may also be used for sharing files between multiple containers,
but this setup needs to be supported by the software running in both of them, otherwise you should expect data corruption to happen.

Based on the volume driver in use, the files may be stored at a network location,
they may be encrypted, compressed, and other possibilities may become available with them.

The official documentation can be read [here](https://docs.docker.com/storage/volumes/).

## Types of volumes

The volumes are primarily differentiated based on how can you refer to them.

Anonymous volumes are ones you cant refer to.  
They are created when it is defined in the container image that certain directories should be stored in a volume, but the user did not set them up as a named volume.  
Although they are not automatically removed along with the container, recreating the container will result in new anonymous volumes being used.  
Their main purpose is simply to indicate which directories should be persisted when the user wants that, but still not interfere when the user intends to create a totally temporary container.

Named volumes can be referred to by their names, and they will be reused as expected when the container is recreated.

Bind mounts are not actually volumes, but it is useful to mention them here.  
They will persist your data like a named volume, but Docker will not list them among volumes.
You define them by specifying a filesystem path instead of a volume name.

## Volume syntax

There are 2 syntaxes available for specifying volumes: the "volume" syntax and the "mount" syntax.

More details about the volume syntax can be read [here](https://docs.docker.com/storage/volumes/#choose-the--v-or---mount-flag).

### The "volume" syntax

The "volume" syntax is more consice, but it allows less detailed configuration.  
All configuration parameters are written on 1 line, in which 3 fields are distinguished, each of them separated by the `:` character.
- The first field is the name of the volume. This must be unique on the host where Docker is running. It is omitted for anonymous volumes.
- The second field is the path **inside of the container** where the volume will be mounted.
- The third field is an optional list of options.

Example volume specification with the "volume" syntax:

|Volume type|Volume specification|
|---:|---|
|named volume|`myapp_data:/var/lib/myapp/`|
|read only named volume|`myapp_data:/var/lib/myapp/:ro`|
|anonymous volume|`/var/log/myapp/`|
|read only anonymous volume|`/var/log/myapp/:ro`|

### The "mount" syntax

The "mount" syntax may be easier to understand, but it is much longer.  
It consists of comma separated key-value pairs.
Keys are `source`, `destination`, `type`, `readonly` and `volume-opt`, several of which has a shortened version.  
`volume-opt` makes it possible to pass configuration options to the selected volume driver. 

Example volume specification wih the "mount" syntax:

|Volume type|Volume specification|
|---:|---|
|named volume|`type=volume,source=myapp_data,destination=/var/lib/myapp/`|
|read only named volume|`type=volume,source=myapp_data,destination=/var/lib/myapp/,readonly`|
|anonymous volume|`type=volume,destination=/var/lib/myapp/`|
|read only anonymous volume|`type=volume,destination=/var/lib/myapp/,readonly`|

## Managing volumes

Volumes are managed by subcommands of the `docker volume` command. These are also documented [here](https://docs.docker.com/engine/reference/commandline/volume/).

Below you will find a brief summary of the volume management commands, but you may also read the official tutorial [here](https://docs.docker.com/network/network-tutorial-standalone/).

### Creating volumes

Volumes can be created with the `docker volume create [options] [name]` command.  
Omitting the `name` parameter will result in an anonymous volume being created.

A specific driver and driver options can be set with the `--driver` and `--driver-opts` options.

### Listing volumes
Existing volumes can be listed with the `docker volume list` command.

### Changing volume settings
Changing volume settings after creation is not possible without recreating it.  
Be sure to backup all data in the volume before you deleting it!

### Deleting volumes
You can delete a volume with the `docker volume remove <name>` command.  
Warning! All data will be lost!

## Volume drivers

The driver of a volume and its parameters can be only set at creation, they cannot be changed later.  
Volume driver specific parameters can be specified with the `volume-opts` key of the "mount" syntax.

Detailed use of volume drivers are documented [here](https://docs.docker.com/storage/volumes/#use-a-volume-driver).

### The `local` driver
This driver will use the capabilities of the Host system for finding a place for the volume's contents.  
The specifics can be configured in driver options, 
which for this driver are documented [here](https://docs.docker.com/engine/reference/commandline/volume_create/#opt).

On Windows, driver options are not supported, and volume data can only be stored on already mounted filesystems.

On Linux based Host systems, the parameterization is similar to using the `mount` command.  
The parameters will be handed over to the `mount` command, which means any type of filesystem can be used that has a mount wrapper installed.  
The following parameters are used:

|Parameter|Meaning|
|---|---|
|device|Location of backing data. Usually path of a block device in case of local filesystems. See the section "Indicating the device and filesystem" in the man page of the `mount` utility for details|
|type|Type of filesystem. See the documentation of `--types` in the man page of the `mount` utility for details|
|o|Comma separated list of mount options. See the documentation of `--options` in the man page of the `mount` utility for details|

On configuration of the `local` driver on Linux systems further explanation can be read [here](https://docs.docker.com/storage/volumes/#block-storage-devices).
Note that the page mentions specifically "block devices", but actually it is relevant independently to the kind of filesystem you use.

Note: it is possible to create named volumes that also work like a bind mount.  
To achieve that, you will need to use the `volume-opt=type=none` and `volume-opt=o=bind` parameters, and also specifiy a bind destination with `volume-opt=device=...`.  
Example: `--mount "type=volume,source=myapp_data,destination=/var/lib/myapp,volume-opt=type=none,volume-opt=o=bind,volume-opt=device=/srv/myapp/data"`.  
It is important to note that the volume data will not be stored at the bound location, it is merely _available_ at that path. Data is stored at the original location, and takes up storage space there, which is unchanged from conventional named volumes.

### 3rd party drivers

There are plenty of volume drivers that are not bundled with Docker, but can be installed separately if needed.
Some of them are listed [on this page](https://docs.docker.com/engine/extend/legacy_plugins/#volume-plugins).
