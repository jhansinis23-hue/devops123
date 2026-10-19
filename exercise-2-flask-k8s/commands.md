\# Exercise 2: Deploy a Flask app on Minikube using kubectl and YAML



\## Objective

Build a Flask app image inside Minikube's Docker daemon, deploy it with a Kubernetes Deployment, and expose it with a NodePort Service.



\## Files

\- app.py: Flask app listening on port 15000

\- Dockerfile: builds the image from python:3.8-slim

\- flask-deployment.yaml: Deployment and NodePort Service



\## Commands used

1\. minikube start --driver=docker

2\. \& minikube -p minikube docker-env --shell powershell | Invoke-Expression

3\. docker build -t flask-app .

4\. kubectl apply -f flask-deployment.yaml

5\. kubectl get deployments

6\. kubectl get pods -l app=flask-app

7\. kubectl describe deployment flask-app

8\. kubectl logs <pod-name>

9\. kubectl get services

10\. curl.exe http://127.0.0.1:15000 (fails: app not exposed yet)

11\. Added the Service section to the YAML and ran kubectl apply -f flask-deployment.yaml again

12\. minikube service flask-app-service --url

13\. Opened the printed URL in the browser



\## Screenshots

\- screenshots/01-pods-running.png (deployment and pod Running)

\- screenshots/03-flask-browser.png (Flask app reachable through the Service)



\## Result

Before the Service existed, port 15000 refused connections. After adding the NodePort Service, minikube service --url gave a local URL and the browser showed "Hello from Flask on Kubernetes!".



\## Q\&A



Q1: What is the purpose of minikube service flask-app-service --url?

A1: To provide the URL for accessing the flask-app-service running in Minikube.



Q2: What happens when you run minikube service flask-app-service --url?

A2: Minikube checks if the service is running, generates a URL, and displays it in the terminal.



Q3: Why is targetPort used in Kubernetes Service configuration?

A3: To specify the port number on which the container is listening.



Q4: What is the difference between port and targetPort in Kubernetes Service configuration?

A4: port is the exposed Service port, while targetPort is the container port.



Q5: How do you access a Flask application running in Minikube?

A5: Use minikube service <service-name> --url to get the access URL.



Q6: Why does the terminal need to remain open when using the Docker driver with Minikube?

A6: The Docker driver needs the terminal open to maintain the tunnel to the service.



Q7: What is the benefit of using the --url flag with the minikube service command?

A7: It provides a convenient way to access the service without manually constructing the URL.



Q8: What command is used to expose a service in Kubernetes?

A8: kubectl expose or kubectl apply -f <service.yaml> is used to expose a service in Kubernetes.



Q9: How does Minikube help in local Kubernetes testing?

A9: Minikube runs a single-node Kubernetes cluster locally, making it easier to test deployments without needing a full Kubernetes environment.



Q10: What is the role of kubectl in this setup?

A10: kubectl is the Kubernetes CLI tool used to interact with the cluster. It allows you to deploy applications, scale them, and manage resources.

