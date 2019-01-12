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
14. docker container logs --tail 5 nginx-test  // last 5 logs
    PS C:\Users\cezar\Desktop\Mastering Docker\Docker\Playground\Chapter 04> docker container logs --tail 5 nginx-test
    172.17.0.1 - - [12/Jan/2019:22:14:32 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36" "-"
    172.17.0.1 - - [12/Jan/2019:22:14:32 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36" "-"
    172.17.0.1 - - [12/Jan/2019:22:14:33 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36" "-"
    172.17.0.1 - - [12/Jan/2019:22:18:16 +0000] "GET / HTTP/1.1" 200 612 "-" "Mozilla/5.0 (Windows NT; Windows NT 10.0; en-AU) WindowsPowerShell/5.1.17134.407" "-"
    172.17.0.1 - - [12/Jan/2019:22:24:32 +0000] "GET / HTTP/1.1" 200 612 "-" "Mozilla/5.0 (Windows NT; Windows NT 10.0; en-AU) WindowsPowerShell/5.1.17134.407" "-"
15. docker container logs --since 2018-08-25T18:00 nginx-test // uses --since flag to show logs from a date till now
    PS C:\Users\cezar\Desktop\Mastering Docker\Docker\Playground\Chapter 04> docker container logs --since 2018-08-25T18:00 nginx-test
    172.17.0.1 - - [12/Jan/2019:21:59:50 +0000] "GET / HTTP/1.1" 200 612 "-" "Mozilla/5.0 (Windows NT; Windows NT 10.0; en-AU) WindowsPowerShell/5.1.17134.407" "-"
    172.17.0.1 - - [12/Jan/2019:22:01:09 +0000] "GET / HTTP/1.1" 200 612 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36" "-"
    172.17.0.1 - - [12/Jan/2019:22:01:09 +0000] "GET /favicon.ico HTTP/1.1" 404 555 "http://localhost:8080/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36" "-"
    2019/01/12 22:01:09 [error] 6#6: *2 open() "/usr/share/nginx/html/favicon.ico" failed (2: No such file or directory), client: 172.17.0.1, server: localhost, request: "GET /favicon.ico HTTP/1.1", host: "localhost:8080", referrer: "http://localhost:8080/"
    172.17.0.1 - - [12/Jan/2019:22:14:32 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36" "-"
    172.17.0.1 - - [12/Jan/2019:22:14:32 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36" "-"
    172.17.0.1 - - [12/Jan/2019:22:14:33 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36" "-"
    172.17.0.1 - - [12/Jan/2019:22:18:16 +0000] "GET / HTTP/1.1" 200 612 "-" "Mozilla/5.0 (Windows NT; Windows NT 10.0; en-AU) WindowsPowerShell/5.1.17134.407" "-"
    172.17.0.1 - - [12/Jan/2019:22:24:32 +0000] "GET / HTTP/1.1" 200 612 "-" "Mozilla/5.0 (Windows NT; Windows NT 10.0; en-AU) WindowsPowerShell/5.1.17134.407" "-"
16. docker container logs -f nginx-test => show all the logs till now + anytrhing that comes in real-time, so the terminal is in -d off.
17. docker container logs --since 2018-08-25T18:00 -t nginx-test  // -t will add a timestamp as per your machine's local time, not container's
    PS C:\Users\cezar\Desktop\Mastering Docker\Docker\Playground\Chapter 04> docker container logs --since 2018-08-25T18:00 -t nginx-test
    2019-01-12T21:59:50.747892600Z 172.17.0.1 - - [12/Jan/2019:21:59:50 +0000] "GET / HTTP/1.1" 200 612 "-" "Mozilla/5.0 (Windows NT; Windows NT 10.0; en-AU) WindowsPowerShell/5.1.17134.407" "-"
    2019-01-12T22:01:09.684956300Z 172.17.0.1 - - [12/Jan/2019:22:01:09 +0000] "GET / HTTP/1.1" 200 612 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36" "-"
    2019-01-12T22:01:09.780417700Z 172.17.0.1 - - [12/Jan/2019:22:01:09 +0000] "GET /favicon.ico HTTP/1.1" 404 555 "http://localhost:8080/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36" "-"

    In the logs above, note the extra first field which is the time stamp.

18. top => shows all the processes currently running in container
    PS C:\Users\cezar\Desktop\Mastering Docker\Docker\Playground\Chapter 04> docker container top nginx-test
    PID                 USER                TIME                COMMAND
    3965                root                0:00                nginx: master process nginx -g daemon off;
    4012                101                 0:00                nginx: worker process

19. stats => shows resources usage in container
    PS C:\Users\cezar\Desktop\Mastering Docker\Docker\Playground\Chapter 04> docker container stats nginx-test
    CONTAINER ID        NAME                CPU %               MEM USAGE / LIMIT     MEM %               NET I/O             BLOCK I/O           PIDS
    2855be1c21e9        nginx-test          0.00%               2.469MiB / 12.19GiB   0.02%               38.4kB / 19.4kB     6.81MB / 0B         2
20. docker container pause nginx-test
    PS C:\Users\cezar\Desktop\Mastering Docker\Docker\Playground\Chapter 04> docker container pause nginx-test
    nginx-test
    PS C:\Users\cezar\Desktop\Mastering Docker\Docker\Playground\Chapter 04> docker container ls
    CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                      PORTS                  NAMES
    f93bf86b1f52        nginx               "nginx -g 'daemon of…"   About an hour ago   Up About an hour            0.0.0.0:8090->80/tcp   nginx-test-1
    2855be1c21e9        nginx               "nginx -g 'daemon of…"   About an hour ago   Up About an hour (Paused)   0.0.0.0:8080->80/tcp   nginx-test
21. docker container unpause nginx-test
    PS C:\Users\cezar\Desktop\Mastering Docker\Docker\Playground\Chapter 04> docker container unpause nginx-test
    nginx-test
    PS C:\Users\cezar\Desktop\Mastering Docker\Docker\Playground\Chapter 04> docker container ls
    CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
    f93bf86b1f52        nginx               "nginx -g 'daemon of…"   About an hour ago   Up About an hour    0.0.0.0:8090->80/tcp   nginx-test-1
    2855be1c21e9        nginx               "nginx -g 'daemon of…"   About an hour ago   Up About an hour    0.0.0.0:8080->80/tcp   nginx-test

22. Lifecycle
    a. Start
        i.  docker container start nginx2 
    b. Stop
        i.  docker container stop nginx2
        ii. docker container stop -t 60 nginx3 // give it 60 seconds
    c. Restart - stops then restart, can send-t flag)
        i.  docker container restart nginx4
        ii. docker container restart -t 60 nginx4
    d. Kill - sends a sIGKILL command immediately
        i.  docker container kill nginx5
    e. Pause - will pause the container
        i.  container pause nginx-test
    f. Unpause - will start the paused contaienr from where it was paused with the original flags it was started with, this is different from start where we send the flag every time
        i.  container unpause nginx-test

23. Removing a container
    a. docker container rm nginx4
    b. docker container rm -f nginx4 => forces a stop and a remove
    c. docker container prune  => removes all stopped containers
