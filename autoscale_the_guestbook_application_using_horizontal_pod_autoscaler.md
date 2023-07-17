# Autoscale the Guestbook application using Horizontal Pod Autoscaler

1. Autoscale the Guestbook deployment using kubectl autoscale deployment. <br>
```kubectl autoscale deployment guestbook --cpu-percent=5 --min=1 --max=10```

2. You can check the current status of the newly-made HorizontalPodAutoscaler, by running:<br>
'''kubectl get hpa guestbook'''

hpa...

3. Open another new terminal and enter the below command to generate load on the app to observe the autoscaling (Please ensure your port-forward command is running. In case you have stopped your application, please run the port-forward command to re-run the application at port 3000.)<br>

```
kubectl run -i --tty load-generator --rm --image=busybox:1.36.0 --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- [<your app URL>](https://education123-3000.theiaopenshift-1-labs-prod-theiaopenshift-4-tor01.proxy.cognitiveclass.ai/)https://education123-3000.theiaopenshift-1-labs-prod-theiaopenshift-4-tor01.proxy.cognitiveclass.ai/; done"
```

4. Run the below command to observe the replicas increase in accordance with the autoscaling:<br>
```kubectl get hpa guestbook --watch```

hpa2...
5. Run the below command to observe the details of the horizontal pod autoscaler:
```kubectl get hpa guestbook```

