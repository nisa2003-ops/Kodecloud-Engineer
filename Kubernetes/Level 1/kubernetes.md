### Task 1
 The Nautilus DevOps team is diving into Kubernetes for application management. One team member has a task to create a pod according to the details below:


Create a pod named pod-nginx using the nginx image with the latest tag. Ensure to specify the tag as nginx:latest.


Set the app label to nginx_app, and name the container as nginx-container.

---

### Solution & Commands

   ```bash
   kubectl run pod-nginx --image=nginx:latest --labels=app=nginx_app --overrides='{
  "spec": {
    "containers": [
      {
        "name": "nginx-container",
        "image": "nginx:latest"
      }
    ]
  }
}'
```

### Task 2
 The Nautilus DevOps team is delving into Kubernetes for app management. One team member needs to create a deployment following these details:


Create a deployment named httpd to deploy the application httpd using the image httpd:latest (ensure to specify the tag)

Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster. 

---

### Solution & Commands

   ```bash
   kubectl create deployment httpd --image=httpd:latest
```

### Task 3
 The Nautilus DevOps team is planning to deploy some micro services on Kubernetes platform. The team has already set up a Kubernetes cluster and now they want to set up some namespaces, deployments etc. Based on the current requirements, the team has shared some details as below:

Create a namespace named dev and deploy a POD within it. Name the pod dev-nginx-pod and use the nginx image with the latest tag. Ensure to specify the tag as nginx:latest.

---

### Solution & Commands

   ```bash
   kubectl create namespace dev

   kubectl run dev-nginx-pod -n dev --image=nginx:latest --overrides='{
  "spec": {
    "containers": [
      {
        "name": "dev-nginx-pod",
        "image": "nginx:latest"
      }
    ]
  }
}'
```

### Task 4
 The Nautilus DevOps team has noticed performance issues in some Kubernetes-hosted applications due to resource constraints. To address this, they plan to set limits on resource utilization. Here are the details:


Create a pod named httpd-pod with a container named httpd-container. Use the httpd image with the latest tag (specify as httpd:latest). Configure the following container-level resource requests and limits for the container:

Requests: Memory: 15Mi, CPU: 100m

Limits: Memory: 20Mi, CPU: 100m 

---

### Solution & Commands

   ```bash
   kubectl run httpd-pod \
  --image=httpd:latest \
  --overrides='{
    "apiVersion": "v1",
    "spec": {
      "containers": [{
        "name": "httpd-container",
        "image": "httpd:latest",
        "resources": {
          "requests": {
            "memory": "15Mi",
            "cpu": "100m"
          },
          "limits": {
            "memory": "20Mi",
            "cpu": "100m"
          }
        }
      }]
    }
  }'
```

### Task 5
 An application currently running on the Kubernetes cluster employs the nginx web server. The Nautilus application development team has introduced some recent changes that need deployment. They've crafted an image nginx:1.19 with the latest updates.


Execute a rolling update for this application, integrating the nginx:1.19 image. The deployment is named nginx-deployment.

Ensure all pods are operational post-update. 

---

### Solution & Commands

   ```bash
   kubectl set image deployment/nginx-deployment nginx-container=nginx:1.19
   kubectl rollout status deployment/nginx-deployment
   kubectl get pods -l app=nginx
```

### Task 6
 Earlier today, the Nautilus DevOps team deployed a new release for an application. However, a customer has reported a bug related to this recent release. Consequently, the team aims to revert to the previous version.


There exists a deployment named nginx-deployment; initiate a rollback to the previous revision.  

---

### Solution & Commands

   ```bash
   kubectl rollout undo deployment/nginx-deployment
```
### Task 7
The Nautilus DevOps team is gearing up to deploy applications on a Kubernetes cluster for migration purposes. A team member has been tasked with creating a ReplicaSet outlined below:


1.Create a ReplicaSet using nginx image with latest tag (ensure to specify as nginx:latest) and name it nginx-replicaset.

2.Apply labels: app as nginx_app, type as front-end.

3.Name the container nginx-container. Ensure the replica count is 4.

---

### Solution & Commands

   ```bash
   cat <<EOF > nginx-replicaset.yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replicaset
  labels:
    app: nginx_app
    type: front-end
spec:
  replicas: 4
  selector:
    matchLabels:
      app: nginx_app
      type: front-end
  template:
    metadata:
      labels:
        app: nginx_app
        type: front-end
    spec:
      containers:
      - name: nginx-container
        image: nginx:latest
EOF
```
```bash
      kubectl apply -f nginx-replicaset.yaml
      kubectl get replicaset nginx-replicaset
      kubectl get pods -l app=nginx_app,type=front-end
```

### Task 8
The Nautilus DevOps team is setting up recurring tasks on different schedules. Currently, they're developing scripts to be executed periodically. To kickstart the process, they're creating cron jobs in the Kubernetes cluster with placeholder commands. Follow the instructions below:

1.Create a cronjob named xfusion.

2.Set Its schedule to something like */5 * * * *. You can set any schedule for now.
    
3.Name the container cron-xfusion.
    
4.Utilize the nginx image with latest tag (specify as nginx:latest).
    
5.Execute the dummy command echo Welcome to xfusioncorp!.
    
6.Ensure the restart policy is OnFailure.

---

### Solution & Commands

   ```bash
   cat <<EOF > cronjob.yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: datacenter
spec:
  schedule: "*/11 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: cron-datacenter
            image: httpd:latest
            command:
            - /bin/sh
            - -c
            - echo Welcome to xfusioncorp!
          restartPolicy: OnFailure
EOF
```

```bash
      kubectl apply -f cronjob.yaml
```

### Task 9
The Nautilus DevOps team is crafting jobs in the Kubernetes cluster. While they're developing actual scripts/commands, they're currently setting up templates and testing jobs with dummy commands. Please create a job template as per details given below:

1.Create a job named countdown-devops.

2.The spec template should be named countdown-devops (under metadata), and the container should be named container-countdown-devops

3.Utilize image ubuntu with latest tag (ensure to specify as ubuntu:latest), and set the restart policy to Never.

4.Execute the command sleep 5


---

### Solution & Commands

   ```bash
  cat <<EOF > job.yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: countdown-devops
spec:
  template:
    metadata:
      name: countdown-devops
    spec:
      containers:
      - name: container-countdown-devops
        image: ubuntu:latest
        command: ["sleep", "5"]
      restartPolicy: Never
EOF
```

```bash
      kubectl apply -f job.yaml
```

