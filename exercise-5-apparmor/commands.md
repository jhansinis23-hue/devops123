\# Exercise 5: Docker Security with AppArmor and Python



\## Objective

Containerize a Flask app, define an AppArmor profile, apply it to a container using the Docker SDK for Python, and test restricted actions.



\## Environment note (important)

AppArmor is a Linux kernel security module. This exercise was run on Windows 11 with Docker Desktop, whose Linux VM does not have AppArmor enabled:



&#x20;   docker info --format "{{.SecurityOptions}}"

&#x20;   \[name=seccomp,profile=builtin name=cgroupns]



There is no "name=apparmor" entry, so apparmor\_parser could not load the profile (Task 3) and the profile was not enforced. The Docker and Python parts ran for real; the results below are the actual outputs.



\## Files

\- app.py: Flask app on port 5000

\- Dockerfile: builds the flask-apparmor image from python:3.8-slim

\- my-apparmor-profile: AppArmor profile (would be saved to /etc/apparmor.d/ on Linux)

\- apply\_apparmor.py: builds and runs the container with the AppArmor security option and inspects it

\- test\_restricted\_actions.py: tries to read /etc/passwd and run /bin/bash inside the container



\## Changes from the lab sheet

\- The profile is named "my-apparmor-profile" (the sheet named it /usr/bin/python3, which would not match the name passed to docker run)

\- The profile is based on Docker's default rules with deny rules added; the sheet's profile only allowed /app/\*\* and would stop Python from starting

\- The scripts remove the container after stopping it, so port 5000 stays free



\## Commands used

1\. docker build -t flask-apparmor .

2\. docker info --format "{{.SecurityOptions}}"

3\. pip install docker

4\. python apply\_apparmor.py

5\. python test\_restricted\_actions.py



\## Screenshots

\- screenshots/01-scripts-output.png (output of both Python scripts)

\- screenshots/02-no-apparmor.png (docker info showing AppArmor is not available)



\## Results

\- apply\_apparmor.py: the container started and HostConfig.SecurityOpt showed \['apparmor=my-apparmor-profile'], so the setting was passed to Docker.

\- test\_restricted\_actions.py: reading /etc/passwd returned exit code 0 and executing /bin/bash returned exit code 0. Both actions were allowed because the Docker Desktop VM has no AppArmor to enforce the profile.

\- On a Linux host with AppArmor, after "sudo apparmor\_parser -r /etc/apparmor.d/my-apparmor-profile", the same actions should be denied (non-zero exit codes such as 126).



\## Questions and Answers



Q1: What is the purpose of using AppArmor with Docker containers?

A1: AppArmor enforces security policies and confines applications to a limited set of resources. With Docker, it limits access to files, networks, and capabilities, adding a layer of security.



Q2: How do AppArmor profiles help secure a Docker container?

A2: Profiles define what a containerized application can or cannot do. They restrict access to sensitive directories, network access, file execution, and capabilities, so a compromised container has limited impact on the host.



Q3: Why is it important to restrict access to sensitive directories such as /etc/ and /var/?

A3: /etc/ holds configuration and account information, and /var/ holds logs and variable data. Restricting access prevents the container from reading or modifying important system files, reducing the risk of security breaches.



Q4: What other capabilities can you restrict using AppArmor profiles?

A4: Network access, binding to specific ports, executing binaries, writing to specific directories, and Linux capabilities such as sys\_admin.



Q5: How can you verify if an AppArmor profile is successfully applied to a Docker container?

A5: Inspect the container (docker inspect or the Docker SDK) and check HostConfig.SecurityOpt for the profile name. To confirm enforcement on Linux, run "aa-status" or test a restricted action and check that it is denied.

