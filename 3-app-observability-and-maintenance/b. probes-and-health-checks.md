## Implement probes and health checks

* [Probes And Health Checks](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/ "Probes And Health Checks")

**1. Create a pod using the <code>wordpress</code> image. Implement a Readiness Probe that executes the command <code>cat readme.html</code>. The kubelet should wait 15 seconds before performing the first check, then run the check every 5 seconds. When did the pod's status change to READY?**

<details><summary>Solution</summary>

<p>

```bash
kubectl run wordpress --image=wordpress --dry-run=client -o yaml > wordpress.yaml
vim wordpress.yaml
```

```YAML
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: wordpress
  name: wordpress
spec:
  containers:
  - image: wordpress
    name: wordpress
    resources: {}
    readinessProbe:              #add
      exec:                      #add
        command:                 #add
        - cat                    #add
        - readme.html            #add
      initialDelaySeconds: 15    #add
      periodSeconds: 5           #add
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```
The Pod's status changes to READY when the readiness probe succeeds

</p>
</details>

<br/>

**2.	Create a Pod using the <code>equivalent/health_check_nginx</code> image. Implement a Liveness Probe that calls the <code>/health-check</code> endpoint using the HTTP GET method on port 80. The kubelet should wait 10 seconds before performing the first check, and then execute the check every 5 seconds.**

<details><summary>Solution</summary>

<p>

```YAML
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: nginx-health
  name: nginx-health
spec:
  containers:
  - image: equivalent/health_check_nginx
    name: nginx-health
    livenessProbe:
      httpGet:
        path: /health-check
        port: 80
      initialDelaySeconds: 10
      periodSeconds: 5
  dnsPolicy: ClusterFirst
  restartPolicy: Always
```

</p>
</details>