\# Exercise 4: Docker Networking with Multiple Containers



\## Objective

Understand Docker bridge networking by connecting a Flask API, a MySQL database, and a Redis cache on one user-defined network.



\## Files

\- app.py: Flask REST API with a /about endpoint on port 5001

\- requirements.txt: Flask==2.3.3

\- Dockerfile: builds the flask-api image from python:3.9-slim



\## Changes from the lab sheet

\- app.run uses host='0.0.0.0' so the published port is reachable from the host

\- MySQL needs MYSQL\_ROOT\_PASSWORD, otherwise the container exits

\- Flask pinned to 2.3.3 for compatibility

\- ping is not in python:3.9-slim, so iputils-ping was installed inside the container



\## Commands used

1\. docker network create --driver bridge my-bridge-net

2\. docker network ls

3\. docker network inspect my-bridge-net

4\. docker build -t flask-api .

5\. docker run -d --name mysql --net=my-bridge-net -e MYSQL\_ROOT\_PASSWORD=root123 mysql:latest

6\. docker run -d --name redis --net=my-bridge-net redis:latest

7\. docker run -d --name flask --net=my-bridge-net -p 5001:5001 flask-api

8\. docker ps

9\. curl.exe http://localhost:5001/about

10\. docker exec -it flask bash, then apt-get update \&\& apt-get install -y iputils-ping

11\. docker exec flask ping -c 3 mysql

12\. docker exec flask ping -c 3 redis

13\. docker stop mysql redis flask

14\. docker rm mysql redis flask

15\. docker network rm my-bridge-net



\## Screenshots

\- screenshots/01-containers-running.png (all three containers Up and the Flask /about response)

\- screenshots/02-ping-by-name.png (flask container pinging mysql and redis by name)



\## Result

All three containers joined my-bridge-net, and the Flask container reached mysql and redis by container name with 0% packet loss. The network inspect output listed all three containers (flask 172.19.0.4, mysql 172.19.0.2, redis 172.19.0.3). After the test, the containers and the network were removed.



\## Questions and Answers



Q1: What is the purpose of the --net flag in docker run?

A1: The --net flag specifies which network the container is connected to.



Q2: How do containers communicate with each other on the same network?

A2: They communicate using container names (resolved by Docker's built-in DNS on user-defined networks) or IP addresses.



Q3: What is the difference between a bridge network and a host network?

A3: A bridge network gives containers their own private network with isolation from the host and from other networks. A host network makes the container share the host's network stack directly, with no isolation.



Q4: How can you expose a container's port to the host machine?

A4: Use the -p flag to publish a port, in the form -p <host-port>:<container-port>, for example -p 5001:5001.

