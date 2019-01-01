# How to

You can test the application by launching a container with the following command:
$ docker container run -d -p 8000:80 --name go-hello-world local:go-helloworld

The application is accessible over a browser and simply increments a counter each time the
page is loaded. To test it on macOS and Linux, you can use the curl command, as follows:
$ curl <http://localhost:8000/>