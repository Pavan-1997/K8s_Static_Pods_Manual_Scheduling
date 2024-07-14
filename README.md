# K8s_Static_Pods_Manual_Scheduling_Labels_Selectors

```
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
```

`kind create cluster --name=pavantest --config=cluster.yaml`

`docker ps`

![image](https://github.com/user-attachments/assets/5c5b3be6-0895-4764-84e7-a6391f8f3759)


Login to the Docker Container - Control Plane

`docker exec -it pavantest-control-plane bash`

`ps -ef|grep kubelete`

![image](https://github.com/user-attachments/assets/df99b621-e5d9-4de7-96e2-a8356e337df5)

The directory where the Static Pods are stored and Kubelet is constantly monitoring is `/etc/kubernetes/manifests`

![image](https://github.com/user-attachments/assets/a12457ce-2f37-4397-8780-c25b49f0b8cd)

Ideally, the scheduler is running when the source file is not changed

`k get pods -n kube-system | grep scheduler`

![image](https://github.com/user-attachments/assets/9e6ce4b3-95e1-498d-93b1-3e5e567cc6c0)

Moving the source file to the temp directory 

`mv kube-scheduler.yaml /tmp`

Now the schedular pod is on longer running

`k get pods -n kube-system | grep scheduler`

![image](https://github.com/user-attachments/assets/c43f04c7-57e5-4282-a82e-efcdacde299a)

Now trying to schedule a new pod to check 

`k run nginx --image=nginx`

`k get pods`

![image](https://github.com/user-attachments/assets/269a2eb0-020c-4f9d-8271-4b0d9976c0d8)

`k describe pod nginx`

![image](https://github.com/user-attachments/assets/13a3624a-8035-4767-a221-f0419f6f2572)

Now moving back the scheduler source file to the actual path `/etc/kubernetes/manifests`

`mv /tmp/kube-scheduler.yaml .`

Now the Nginx pod is Running 

`k get pods -w`

![image](https://github.com/user-attachments/assets/963cf55d-8401-4445-8a75-58748cfa30d9)

---

## Manual Scheduling

`k run nginx --image=nginx --dry-run=client -o yaml > nginx.yaml`

In this File we add Manual Scheduling as the one updated in GitHUb

Ideally, the scheduler is running when the source file is not changed

`k get pods -n kube-system | grep scheduler`

![image](https://github.com/user-attachments/assets/9e6ce4b3-95e1-498d-93b1-3e5e567cc6c0)

Moving the source file to the temp directory 

`mv kube-scheduler.yaml /tmp`

Now the schedular pod is on longer running

`k get pods -n kube-system | grep scheduler`

![image](https://github.com/user-attachments/assets/c43f04c7-57e5-4282-a82e-efcdacde299a)

Now trying to schedule a new pod with Manual Scheduling 

`k apply -f nginx1.yaml`

`k get pods`

![image](https://github.com/user-attachments/assets/e75cf6d8-5f65-4090-afbb-039aa6bea6d7)
