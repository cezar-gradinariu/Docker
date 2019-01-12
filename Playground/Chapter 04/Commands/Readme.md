# Notes

1. docker container run -d --name nginx-test -p 8080:80 nginx // launching nginx on port 8080 with -d detach-all flag
2. curl http://localhost:8080 // html response seen in the terminal
    PS C:\Users\cezar\Desktop\Mastering Docker\Docker\Playground\Chapter 04> curl http://localhost:8080
    StatusCode        : 200
    StatusDescription : OK
    ............<HTML content>...................
3. in browser http://localhost:8080 // html response is not seen the terminal => because of -d
4. docker container run --name nginx-test-1 -p 8090:80 nginx // different name and no -d flag => you will see that the cli is no longer available
5. in browser/postman call http://localhost:8080 and you will see in the cli terminal the following log
    PS C:\Users\cezar\Desktop\Mastering Docker\Docker\Playground\Chapter 04> docker container run --name nginx-test-1 -p 8090:80 nginx
    172.17.0.1 - - [12/Jan/2019:22:02:53 +0000] "GET / HTTP/1.1" 200 612 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36" "-"
    172.17.0.1 - - [12/Jan/2019:22:02:53 +0000] "GET /favicon.ico HTTP/1.1" 404 555 "http://localhost:8090/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36" "-"
    2019/01/12 22:02:53 [error] 6#6: *1 open() "/usr/share/nginx/html/favicon.ico" failed (2: No such file or directory), client: 172.17.0.1, server: localhost, request: "GET /favicon.ico HTTP/1.1", host: "localhost:8090", referrer: "http://localhost:8090/"
6. ctrl-C to exit the block
7. PS C:\Users\cezar\Desktop\Mastering Docker\Docker\Playground\Chapter 04> docker container ls
    CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
    f93bf86b1f52        nginx               "nginx -g 'daemon of…"   5 minutes ago       Up 5 minutes        0.0.0.0:8090->80/tcp   nginx-test-1
    2855be1c21e9        nginx               "nginx -g 'daemon of…"   8 minutes ago       Up 8 minutes        0.0.0.0:8080->80/tcp   nginx-test
   As you can see the container is still running, in the background
8. curl http://localhost:8090
    StatusCode        : 200
    StatusDescription : OK
    ............<HTML content>...................

9. docker container attach nginx-test // will attach to nginx-test running container , as if we had -d off, so your nginx logs will be on terminal
10. in browser http://localhost:8080
11. in terminal you will now see
    PS C:\Users\cezar\Desktop\Mastering Docker\Docker\Playground\Chapter 04> docker container attach nginx-test
    172.17.0.1 - - [12/Jan/2019:22:14:32 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36" "-"
12. docker container exec nginx-test cat /etc/debian_version // exec spwans a secondary process within the container and you can interact with it
    PS C:\Users\cezar\Desktop\Mastering Docker\Docker\Playground\Chapter 04> docker container exec nginx-test cat /etc/debian_version
    9.6
13. docker container exec -i -t nginx-test /bin/bash
    PS C:\Users\cezar\Desktop\Mastering Docker\Docker\Playground\Chapter 04> docker container exec -i -t nginx-test /bin/bash
    root@2855be1c21e9:/# apt-get upgrade
    Reading package lists... Done
    Building dependency tree
    Reading state information... Done
    Calculating upgrade... Done
    0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
    root@2855be1c21e9:/# exit
    exit