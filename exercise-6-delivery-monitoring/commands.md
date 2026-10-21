\# Exercise 6: Real-Time Operations Monitoring and Alerting



\## Objective

Simulate delivery metrics with Python, scrape them with Prometheus, visualise them in Grafana, raise alerts on thresholds, and automate the build with a Jenkins pipeline.



\## Files

\- delivery\_metrics.py: Python app exposing delivery metrics on port 8000 (prometheus\_client)

\- prometheus.yml: Prometheus scrape configuration

\- alert\_rules.yml: alert rules (HighPendingDeliveries, HighAverageDeliveryTime)

\- Dockerfile: containerises the metrics app for the Jenkins pipeline

\- Jenkinsfile: pipeline (pre-check, workspace setup, image build, run)



\## Changes from the lab sheet (Windows / Docker Desktop)

\- Prometheus is published with -p 9090:9090 (--network=host does not work on Docker Desktop for Windows)

\- Prometheus scrapes host.docker.internal:8000 instead of 172.17.0.1:8000

\- Grafana data source URL is http://host.docker.internal:9090

\- Scrape and evaluation interval set to 5s so alerts appear quickly

\- The alert rules file is alert\_rules.yml everywhere (the sheet mixed alerts\_rules.yml and alert\_rules.yml)

\- Jenkins runs on port 8090 because ports 8080 and 8081 are in use by other containers

\- Jenkins was started as root with the Docker socket mounted, the Docker CLI installed inside it, and the project folder mounted read-only at /source

\- The Jenkinsfile copies from /source (instead of a placeholder path), uses a Dockerfile added for this exercise, publishes the pipeline container on port 8001 (8000 is used by the local app), and drops the Prometheus and Grafana stage because they were already running



\## Commands used

1\. pip install prometheus-client

2\. python delivery\_metrics.py

3\. curl.exe -s http://localhost:8000/metrics | findstr deliver

4\. docker run -d --name prometheus -p 9090:9090 -v prometheus.yml and alert\_rules.yml mounted prom/prometheus

5\. docker run -d --name grafana -p 3000:3000 grafana/grafana

6\. docker run -d -p 8090:8080 -p 50000:50000 --name jenkins -u root -v jenkins\_home:/var/jenkins\_home -v /var/run/docker.sock:/var/run/docker.sock -v <project>:/source:ro jenkins/jenkins:lts

7\. docker exec jenkins bash -c "apt-get update \&\& apt-get install -y docker.io"

8\. Created a Pipeline job in Jenkins with the Jenkinsfile and ran Build Now

9\. curl.exe -s http://localhost:8001/metrics | findstr deliver

10\. Changed pending to random.randint(50, 100), restarted the app and Prometheus, checked the alerts page



\## Screenshots

\- screenshots/02-metrics-endpoint.png (metrics endpoint)

\- screenshots/04-prometheus-query.png (pending\_deliveries query)

\- screenshots/05-prometheus-alerts.png (alerts firing)

\- screenshots/07-grafana-dashboard.png (Delivery Monitoring dashboard with 4 panels)

\- screenshots/08-jenkins-console.png (Jenkins pipeline console output)

\## Result

The Python app exposed total, pending and on-the-way deliveries and average delivery time. Prometheus scraped it (target UP) and both alert rules fired. The Grafana dashboard showed four panels: Total Deliveries, Pending Deliveries, On-the-Way Deliveries and Average Delivery Time (sum / count, about 30 seconds). The Jenkins pipeline built the image and started a metrics container serving the same metrics on port 8001. After raising pending deliveries to 50-100, HighPendingDeliveries fired with values above 50.

