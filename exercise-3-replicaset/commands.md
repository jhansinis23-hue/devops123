\# Exercise 3: Scaling a Flask app with ReplicaSets



\## Objective

Run a Flask "flash sale" app as a ReplicaSet on a single-node Minikube cluster, scale it, and observe self-healing and pod distribution.



\## Files

\- app.py: Flask app with /, /buy and /health endpoints (shows which pod served the request)

\- Dockerfile: builds the flashsale:1.0 image (python:3.11-slim, gunicorn on port 5000)

\- flashsale-replicaset.yaml: ReplicaSet (3 replicas, probes, resource limits) and ClusterIP Service



\## Commands used

1\. minikube start --driver=docker --nodes=1

2\. \& minikube -p minikube docker-env --shell powershell | Invoke-Expression

3\. docker build -t flashsale:1.0 .

4\. kubectl apply -f flashsale-replicaset.yaml

5\. kubectl get pods

6\. kubectl get rs

7\. kubectl scale rs flashsale-rs --replicas=5

8\. kubectl get rs

9\. kubectl get pods

10\. kubectl delete pod flashsale-rs-k7cbt

11\. kubectl get pods

12\. kubectl get pods -o wide



\## Screenshots

\- screenshots/01-scaled-to-5.png (ReplicaSet scaled from 3 to 5 pods)

\- screenshots/02-delete-and-wide.png (deleted pod replaced automatically, all pods on node minikube)



\## Result

The ReplicaSet started with 3 pods and scaled to 5 with one command. When a pod was deleted, a new one was created automatically to keep 5 running. All 5 pods ran on the single node "minikube".



\## Q\&A



Q1: What is the initial number of replicas in the ReplicaSet?

A1: 3



Q2: How many pods are running after applying the ReplicaSet configuration?

A2: 3



Q3: What happens when you scale the ReplicaSet to 5 replicas?

A3: Kubernetes creates 2 additional pods to meet the desired number of replicas (5). The ReplicaSet then has 5 running pods.



Q4: What happens when you delete one pod?

A4: Kubernetes automatically creates a new pod to replace the deleted one, maintaining the desired number of replicas (5).



Q5: How does Kubernetes maintain the desired number of replicas?

A5: Kubernetes continuously compares the number of running pods to the desired number of replicas. If there is a difference, it creates or deletes pods to match the desired state.



Q6: How many nodes are running?

A6: 1



Q7: Where are the pods running with respect to nodes?

A7: All 5 pods are running on the single node (minikube).

