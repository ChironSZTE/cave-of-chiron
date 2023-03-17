# Docker Networks

Use of Docker networks are required for containers that need any kind of network access.  
They appear in the Host system as network interfaces, but containers only see the the networks to which they are connected.
A container may be connected to multiple networks at the same time.
  
When specific containers should not be able to access the services of each other, separate networks can be made for them.

## Types of networks

Docker networks work differently based on what network driver they use. They are explained in detail [on this page](https://docs.docker.com/network/#network-drivers), but the most important ones are discussed below.

## Preinstalled networks

3 special networks exist by default, after installing Docker:

|Network name|Network driver|Purpose|
|---|---|---|
|bridge|[bridge](https://docs.docker.com/network/bridge/)|Provides a common network for containers, with access to the Host's LAN, and internet though it|
|host|[host](https://docs.docker.com/network/host/)|Not a separate network, but symbolizes all of the network interfaces of the Host system. Removes network isolation|
|none|[none](https://docs.docker.com/network/none/)|A dummy network for not providing network access. Containers connected to this network cannot reach each other|
 
By default, every new container is connected to the "bridge" network.

## Managing networks

Management of Docker networks are done with subcommands of the `docker network` command. These are also documented [here](https://docs.docker.com/engine/reference/commandline/network/).

Below you will find a brief summary of the network management commands, but you may also read the official tutorial about using bridge networks [here](https://docs.docker.com/network/network-tutorial-standalone/).

### Listing networks

You can list networks with the `docker networks ls` command:
```
$ sudo docker network ls
NETWORK ID     NAME                DRIVER    SCOPE
269abdce888a   bridge              bridge    local
4b6a1a4cc4be   gitea_gitea         bridge    local
aeb97d2296a4   host                host      local
436c3627b463   none                null      local
2150552f25c0   smb_default         bridge    local
fe1c464e1c19   t2-proxy            bridge    local
1a656e0f077c   traefik_default     bridge    local
1f08b853abde   vikunja_default     bridge    local
184d18342c35   wireguard_default   bridge    local
```

### Creating networks

You can create a network with the `docker network create [options] <name>` command:
```
docker network create my_network
```

Useful parameters:

|Option|Meaning|
|---|---|
|-d \<driver name>, --driver \<driver name>|Specify the network driver to use|
|--internal|The network will not provide access to the Host's LAN and to the internet|

### Modifying networks

Modification of networks is not possible. If you changed your mind about the confgiuration of a network, you will need to delete and recreate it.

### Deleting networks

You can delete a network with the `docker network rm <network name>` command.
Before you delete the network, you will need to disconnect all containers from it, either with stopping them, or by using `docker network disconnect <network name> <container name or id>`.

### Advanced network management commands

It is possible to manage the network connections of containers while they are running.  
You can use the `docker network connect <network name> <container name or id>` and `docker network disconnect <network name> <container name or id>` command for that.

For printing the current state and configuration of a network, you can use the `docker network inspect <network name>` command.  
This may be useful for debugging a network problem.
