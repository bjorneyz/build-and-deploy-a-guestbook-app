# Perform Rolling Updates and Rollbacks on the Guestbook application

1. Please update the title and header in index.html to any other suitable title and header like <Your name> Guestbook - v2 & Guestbook - v2.<br>
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta content="text/html; charset=utf-8" http-equiv="Content-Type">
    <meta charset="utf-8">
    <meta content="width=device-width" name="viewport">
    <link href="style.css" rel="stylesheet">
    <title>Bjørnar's Guestbook - v2</title>
  </head>
  <body>
    <div id="header">
      <h1>Guestbook - v2</h1>
    </div>

    <div id="guestbook-entries">
      <link href="https://afeld.github.io/emoji-css/emoji.css" rel="stylesheet">
      <p>Waiting for database connection... <i class='em em-boat'></i></p>
      
    </div>

    <div>
      <form id="guestbook-form">
        <input autocomplete="off" id="guestbook-entry-content" type="text">
        <a href="#" id="guestbook-submit">Submit</a>
      </form>
    </div>

    <div>
      <p><h2 id="guestbook-host-address"></h2></p>
      <p><a href="env">/env</a>
      <a href="info">/info</a></p>
    </div>
    <script src="jquery.min.js"></script>
    <script src="script.js"></script>
  </body>
</html>
```

2. Run the below command to build and push your updated app image:<br>
```docker build . -t us.icr.io/$MY_NAMESPACE/guestbook:v1 && docker push us.icr.io/$MY_NAMESPACE/guestbook:v1```

3. Update the values of the CPU in the deployment.yml to cpu: 5m and cpu: 2m as below:<br>
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
            cpu: 5m
          requests:
            cpu: 2m  

```

4. Apply the changes to the deployment.yml file.<br>
```kubectl apply -f deployment.yml```

5. Open a new terminal and run the port-forward command again to start the app:<br>
```kubectl port-forward deployment.apps/guestbook 3000:3000```



