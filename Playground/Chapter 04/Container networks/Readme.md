# NETWORKS

Networks allow multiple containers to communicate with each other.

1. Creating a network: docker network create NETWORK_NAME
    ```bash
    PS C:\Users\cezar\Desktop\Mastering Docker\Docker> docker network create mynet
    c8e0fee9efb86194f9ce0b4142b7f9ae6c28197a5eeb6daf7db8abbb8ca53810
    ```
2. Listing existing networks: docker network ls
    ```bash
    PS C:\Users\cezar\Desktop\Mastering Docker\Docker> docker network ls
    NETWORK ID          NAME                DRIVER              SCOPE
    c440a4e53906        bridge              bridge              local
    3774f8643bc4        host                host                local
    c8e0fee9efb8        mynet               bridge              local
    50226636668d        none                null                local
    ```

3. Start a container in the network : use --network flag followed by your NETWORK_NAME
    ```bash
    PS C:\Users\cezar\Desktop\Mastering Docker\Docker> docker container run -d --name redis --network mynet redis:alpine
    2868e3cecd78b15f20d69335086dee70cd586ac16a00b9a3d095b64714ef3203
    ```

4. Inspect network: docker network inspect NETWORK_NAME
    ```json
    PS C:\Users\cezar\Desktop\Mastering Docker\Docker> docker network inspect mynet
    [
        {
            "Name": "mynet",
            "Id": "c8e0fee9efb86194f9ce0b4142b7f9ae6c28197a5eeb6daf7db8abbb8ca53810",
            "Created": "2019-01-13T03:06:37.9019102Z",
            "Scope": "local",
            "Driver": "bridge",
            "EnableIPv6": false,
            "IPAM": {
                "Driver": "default",
                "Options": {},
                "Config": [
                    {
                        "Subnet": "172.18.0.0/16",
                        "Gateway": "172.18.0.1"
                    }
                ]
            },
            "Internal": false,
            "Attachable": false,
            "Ingress": false,
            "ConfigFrom": {
                "Network": ""
            },
            "ConfigOnly": false,
            "Containers": {
                "2868e3cecd78b15f20d69335086dee70cd586ac16a00b9a3d095b64714ef3203": {
                    "Name": "redis",
                    "EndpointID": "264435e65e2c8d7a77c94f250e0dc0a39740a6cd79ae65043c1ec8165acb0f1f",
                    "MacAddress": "02:42:ac:12:00:02",
                    "IPv4Address": "172.18.0.2/16",
                    "IPv6Address": ""
                }
            },
            "Options": {},
            "Labels": {}
        }
    ]
    ```

    As you can see, there is a section that will show you the containers that are linked to this network, in this case si redis only.

5. Remove a network - can be done in a few steps, but it requires the contianer to stop and then we prune the networks
    * Stop container
         ```bash
        PS C:\Users\cezar\Desktop\Mastering Docker\Docker> docker container ls
        CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                PORTS                  NAMES
        2868e3cecd78        redis:alpine        "docker-entrypoint.s…"   23 minutes ago      Up 23 minutes         6379/tcp               redis
        f93bf86b1f52        nginx               "nginx -g 'daemon of…"   5 hours ago         Up 5 hours            0.0.0.0:8090->80/tcp   nginx-test-1
        2855be1c21e9        nginx               "nginx -g 'daemon of…"   6 hours ago         Up 6 hours (Paused)   0.0.0.0:8080->80/tcp   nginx-test
        PS C:\Users\cezar\Desktop\Mastering Docker\Docker> docker container stop redis
        redis
        ```
    * Call network prune with interaction
        ```bash
        PS C:\Users\cezar\Desktop\Mastering Docker\Docker> docker network prune
        WARNING! This will remove all networks not used by at least one container.
        Are you sure you want to continue? [y/N] y
        Deleted Networks:
        mynet
        ```
        Or,
    * Without interaction by using the -f , --force flag
        ```bash
        PS C:\Users\cezar\Desktop\Mastering Docker\Docker> docker network create  mynet
        faed949cc351f10ba90b7ada233780cebe989ad390f459e2066884f099284cc7
        PS C:\Users\cezar\Desktop\Mastering Docker\Docker> docker network prune -f
        Deleted Networks:
        mynet
        ```
    * A full cleanup in a single sweep
        ```bash
        docker container rm -f redis | docker network prune -f
        ```
