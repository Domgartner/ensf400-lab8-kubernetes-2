## steps and outputs meeting the requirements

First, to initialize Minikube, run:
```bash
$ minikube start
```

Next, enable the ingress addon by running the following command:
```bash
$ minikube addons enable ingress
```

Once the minikube is initialized, we can begin deploying all the yaml files.
```bash
$ kubectl apply -f nginx-dep.yaml
$ kubectl apply -f nginx-dep.yaml
$ kubectl apply -f nginx-configmap.yaml
$ kubectl apply -f nginx-svc.yaml
$ kubectl apply -f nginx-ingress.yaml 
$ kubectl apply -f app-1-svc.yaml
$ kubectl apply -f app-1-ingress.yaml
$ kubectl apply -f app-1-dep.yaml
$ kubectl apply -f app-2-svc.yaml
$ kubectl apply -f app-2-ingress.yaml
$ kubectl apply -f app-2-dep.yaml
```

This will deploy the necessary resources to Minikube. You can verify the deployment by checking the status of the deployments, services, and ingresses:
```bash
$ kubectl get deployments
$ kubectl get services
$ kubectl get ingress
```
The commands above should return responses similar to:
![Alt text](https://github.com/Domgartner/ensf400-lab8-kubernetes-2/blob/main/assignment3/screenshots/Screenshot%202024-04-08%20at%201.20.29%E2%80%AFPM.png)

![Alt text](https://github.com/Domgartner/ensf400-lab8-kubernetes-2/blob/main/assignment3/screenshots/Screenshot%202024-04-08%20at%201.20.46%E2%80%AFPM.png)

![Alt text](https://github.com/Domgartner/ensf400-lab8-kubernetes-2/blob/main/assignment3/screenshots/Screenshot%202024-04-08%20at%201.21.07%E2%80%AFPM.png)

You can also check all of the services running through the command:
```bash
$ kubectl get all
```

Once this is completed and all services and deployments are running as intended, we can test the application to see if nginx is load balancing the deployments:
```bash
$ curl http://$(minikube ip)/
```

Running the curl command, the following outputs should occur (randomly):
![Alt text](https://github.com/Domgartner/ensf400-lab8-kubernetes-2/blob/main/assignment3/screenshots/Screenshot%202024-04-08%20at%201.02.48%E2%80%AFPM.png)

App-2-ingress is configured for a canary deployment. Thus, the traffic is split between app-1 and app-2 by weight 70% for app-1 and 30% for app-2.
The image above shows a near 70-30 split in traffic.

### Cleanup and teardown

To clean up, run the following commands:
```bash
$ kubectl delete -f nginx-dep.yaml
$ kubectl delete -f nginx-svc.yaml
$ kubectl delete -f nginx-configmap.yaml
$ kubectl delete -f app-1-dep.yaml
$ kubectl delete -f app-2-dep.yaml
$ kubectl delete -f app-1-svc.yaml
$ kubectl delete -f app-2-svc.yaml
$ kubectl delete -f app-1-ingress.yaml
$ kubectl delete -f app-2-ingress.yaml
```

Finally, stop minikube:
```bash
$ minikube delete
```
