# Build the guestbook app

To begin, we will build and deploy the web front end for the guestbook app.

1. Change to the v1/guestbook directory:<br> ```cd v1/guestbook```

2. Dockerfile incorporates a more advanced strategy called multi-stage builds, so feel free to read more about that here. Complete the Dockerfile with the necessary Docker commands to build and push your image. The path to this file is guestbook/v1/guestbook/Dockerfile.

```
# Please choose from the below set of Docker commands to complete your Dockerfile:

# COPY
# RUN
# ADD
# EXPOSE
# FROM
# CMD
# PUSH
# PULL
# WORKDIR


FROM golang:1.15 as builder
RUN go get github.com/codegangsta/negroni
RUN go get github.com/gorilla/mux 
RUN go get github.com/xyproto/simpleredis/v2
COPY main.go .
RUN go build main.go

FROM ubuntu:18.04
COPY --from=builder /go/main /app/guestbook
COPY public/index.html /app/public/index.html
COPY public/script.js /app/public/script.js
COPY public/style.css /app/public/style.css
COPY public/jquery.min.js /app/public/jquery.min.js

WORKDIR /app
CMD ["./guestbook"]
EXPOSE 3000
```

3. Export your namespace as an environment variable so that it can be used in subsequent commands.<br>
```export MY_NAMESPACE=sn-labs-$USERNAME```

4. Build the guestbook app using the Docker Build command.<br>
```docker build . -t us.icr.io/$MY_NAMESPACE/guestbook:v1```

5. Push the image to IBM Cloud Container Registry.<br>
```docker push us.icr.io/$MY_NAMESPACE/guestbook:v1```

6. Verify that the image was pushed successfully.<br>
```ibmcloud cr images```

Add photo crimages.

8. Open the deployment.yml file in the v1/guestbook directory & view the code for the deployment of the application:<br>
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: guestbook
  labels:
    app: guestbook 
spec:
  replicas: 1
  selector:
    matchLabels:
      app: guestbook
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: guestbook
    spec:
      containers:
      - image: us.icr.io/sn-labsassets/guestbook:v1
        imagePullPolicy: Always
        name: guestbook
        ports:
        - containerPort: 3000
          name: http
        resources:
          limits:
            cpu: 50m
          requests:
            cpu: 20m  
  replicas: 1
```

8. Apply the deployment using:<br>
```kubectl apply -f deployment.yml```

9. Open a New Terminal and enter the below command to view your application:<br>
```kubectl port-forward deployment.apps/guestbook 3000:3000```

### Preview:
