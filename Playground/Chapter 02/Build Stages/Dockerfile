FROM golang:latest as build
WORKDIR /go-http-hello-world/
RUN go get -d -v golang.org/x/net/html
ADD https://raw.githubusercontent.com/geetarista/go-http-hello-world/master/hello_world/hello_world.go ./hello_world.go
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o app .

FROM scratch
COPY --from=build /go-http-hello-world/app .
CMD ["./app"]