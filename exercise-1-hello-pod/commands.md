\# Exercise 1: Hello Pod (nginx on Minikube)



\## Objective

Deploy an nginx pod on a local Kubernetes cluster (Minikube) and access it through a NodePort service.



\## Commands used



1\. Start the cluster:

&#x20;  minikube start --driver=docker



2\. Create the pod:

&#x20;  kubectl run hello-k8s --image=nginx --port=80



3\. Verify the pod:

&#x20;  kubectl get pods



4\. Expose the pod as a NodePort service:

&#x20;  kubectl expose pod hello-k8s --type=NodePort --port=80



5\. Open the app in the browser:

&#x20;  minikube service hello-k8s



\## Screenshots

\- screenshots/01-pod-service-tunnel.png (pod created, service exposed, tunnel started)

\- screenshots/02-nginx-browser.png (nginx welcome page)



\## Result

The service was exposed as NodePort on port 80 and the nginx welcome page opened in the browser, confirming the pod is reachable.

