# GoCurrentTimeDockerized
Dockerized simple server written in Go that shows the current time.

## Dockerfile
It doesn't come easier than this:

```
FROM golang:onbuild
EXPOSE 8080
```

## Go <code>
```
package main

import (
    "fmt"
    "net/http"
    "time"
)

func main() {
    http.HandleFunc("/", handler)
    http.ListenAndServe(":8080", nil)
}

func handler(w http.ResponseWriter, r *http.Request) {
    curTime := time.Now().Format("02.01.2006 15:04:05")

    fmt.Fprintf(w, "%s", curTime)
}
```

Only important thing to note here is that we're listening on port 8080, which we need to set in the Dockerfile through `EXPOSE`.

## Steps on how to make this yourself

### Go <code>
+ Create the Go app (for testing purposes, you may use the same exact code)
+ Test it locally with `go run curtime.go`
+ Open your browser and go to [http://localhost:8080](http://localhost:8080)
+ You should see the current time displayed

### Docker
+ Create a `Dockerfile` with that content (Make sure you set the correct port in case you'll be using a different one)
+ Inside the source folder (where you have `curtime.go` and `Dockerfile`) run `docker build -t yourUserName/curtime .` - this will create the image
+ You can check if the image was created with `docker images`
+ Run the image in a container: `docker run -d -p 8000:8080 yourUserName/curtime`
+ Access the app via your browser on port `8000`. I used 8000 and 8080 intentionally so that it's easier to explain that the 8000 port is the one on your computer and the 8080 is the port to which we're 'binding' to

Few other commands:
+ `docker ps` will list all the running containers. By adding the `-a` switch, you'll see even the stopped ones
+ `docker stop` - stop the running container
+ `docker rm containerID` - delete the container with id `containerID`
+ `docker images` - list the images that are on your machine
+ `docker rmi imageID` - delete the image with id `imageID`