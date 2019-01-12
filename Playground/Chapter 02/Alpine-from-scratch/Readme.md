# NOTES

1. Original command to build an image
    $ docker image build --file <path_to_Dockerfile> --tag <REPOSITORY>:<TAG> .

2. cd into the folder
3. run command to build image:
    docker image build -t local:scratch-alpine .

4. run it
    a. docker container run -it --name alpine-test alpine:latest
    or, with shell directly
    b. docker container run -it --name alpine-test alpine:latest /bin/sh
        i. run apk --help
        ii. have fun with some apk install, upgrade, and so on + bash commands