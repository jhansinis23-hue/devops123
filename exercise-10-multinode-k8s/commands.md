# Exercise 10: Multi-Node Kubernetes Cluster with Multiple Applications and ReplicaSets

## Objective
Run two applications on a 3-node Minikube cluster, with each replica placed on a different node for availability and fault tolerance.

## Applications
- Product Catalog (product_catalog.py): GET /products, 2 replicas
- Shopping Cart (shopping_cart.py): GET and POST /cart, 3 replicas

## Files
- product_catalog.py, shopping_cart.py: the two Flask applications
- Dockerfile.product, Dockerfile.shopping: image definitions
- product_catalog_deployment.yaml, shopping_cart_deployment.yaml: Deployments (2 and 3 replicas) with pod anti-affinity on kubernetes.io/hostname
- product_catalog_service.yaml, shopping_cart_service.yaml: NodePort Services

## Commands used
1. minikube start --nodes 3 -p devops-multinode --driver=docker
2. kubectl get nodes
3. docker build -t product-catalog:latest -f Dockerfile.product .
4. docker build -t shopping-cart:latest -f Dockerfile.shopping .
5. minikube -p devops-multinode image load product-catalog:latest
6. minikube -p devops-multinode image load shopping-cart:latest
7. kubectl apply -f product_catalog_deployment.yaml
8. kubectl apply -f shopping_cart_deployment.yaml
9. kubectl apply -f product_catalog_service.yaml
10. kubectl apply -f shopping_cart_service.yaml
11. kubectl get pods -o wide
12. kubectl port-forward service/product-catalog-service 8081:80
13. kubectl port-forward service/shopping-cart-service 8082:80
14. curl.exe http://127.0.0.1:8081/products
15. curl.exe http://127.0.0.1:8082/cart (GET, then POST with the JSON read from a file)

## Changes from the lab sheet
- The --force flag was not used (it is only for running as root on Linux)
- The registry add-on was not needed; minikube image load copies the images to every node
- The shopping cart Deployment namespace was changed from devops-exercise to default (that namespace does not exist)
- Services were tested with kubectl port-forward on fixed ports (Windows with the Docker driver)
- The POST body was sent from a JSON file (-d "@item.json") because PowerShell strips the quotes from inline JSON

## Screenshots
- screenshots/01-nodes.png (3 nodes Ready)
- screenshots/02-pods-wide.png (pods spread across the three nodes)

## Result
All three nodes were Ready. The 2 product-catalog pods ran on devops-multinode-m02 and devops-multinode-m03, and the 3 shopping-cart pods ran on devops-multinode, devops-multinode-m02 and devops-multinode-m03, so no two replicas of the same application share a node. The product list returned all three products, the cart started empty, and an item added with POST was returned by the next GET.

## Questions
Q: Why does each replica run on a different node?
A: The podAntiAffinity rule (requiredDuringSchedulingIgnoredDuringExecution) on kubernetes.io/hostname stops the scheduler from placing two pods with the same app label on the same node.

Q: What does this give us?
A: High availability: if one node fails, the other replicas on the remaining nodes keep serving requests.
