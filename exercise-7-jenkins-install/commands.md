# Exercise 7: Introduction to Continuous Integration (CI) and Jenkins Installation

## What is Continuous Integration (CI)?
CI is a practice where developers frequently merge code changes into a shared repository. Each change triggers an automated build and automated tests, so integration problems are found early and developers get quick feedback.

## Benefits of CI
- Early bug detection
- Better collaboration on a shared codebase
- Faster development cycles through automated builds and tests
- Higher code quality and fewer regressions

## How CI works
1. A developer pushes code to a version control system (for example Git).
2. The CI server (for example Jenkins or GitLab CI) detects the change and starts a build.
3. The code is built and dependencies are resolved.
4. Automated tests run.
5. The result (success or failure) is reported back to the developer.

## Example CI tools
GitLab CI/CD, CircleCI, Travis CI, Bamboo, TeamCity, Azure DevOps Pipelines, GitHub Actions, Spinnaker, Buildkite, Drone.

## What is Jenkins?
Jenkins is an open-source automation server used to build, test and deploy software. It supports CI/CD, has a large plugin ecosystem, and can run builds on multiple machines (controller and agents).

Key concepts: Job/Project, Build, Pipeline, Plugins, Nodes.

## Installation (Docker)
Lab command:
    docker run -d --name jenkins -p 8080:8080 -p 50000:50000 jenkins/jenkins:lts

On my machine port 8080 is used by another container, so Jenkins is published on port 9090:
    docker run -d -p 9090:8080 -p 50000:50000 --name jenkins -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts

Check that it is running:
    docker ps --filter name=jenkins

Get the initial admin password (only available before first-time setup is completed):
    docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword

Then open http://localhost:9090, paste the password, install the suggested plugins, create the admin user and start using Jenkins.

## Screenshots
- screenshots/01-jenkins-container.png (Jenkins container running)
- screenshots/02-jenkins-dashboard.png (Jenkins home page)

## Result
Jenkins was installed with Docker, set up with the suggested plugins, and the dashboard was reachable at http://localhost:9090.
