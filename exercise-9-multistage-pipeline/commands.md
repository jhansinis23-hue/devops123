# Exercise 9: Jenkins Multi-Stage Pipeline (Deploying a Python Application)

## Objective
Create a Jenkins Pipeline that builds, tests, deploys and runs a small Python Flask application, with the pipeline definition stored in Git.

## Repository used
https://github.com/jhansinis23-hue/devops-sample-code (public)

Files in that repository for this exercise:
- app.py: Flask app returning "Hello, Jenkins Multi-Stage Pipeline!"
- requirements.txt: flask==2.1.2
- test_app.py: unit test for the home page
- Jenkinsfile: the pipeline definition

## Pipeline stages
1. Build: prints the Python and Flask versions to confirm the environment is ready
2. Test: python3 -m unittest discover -s .
3. Deploy: copies app.py to python-app-deploy in the workspace
4. Run Application: starts the deployed app in the background with nohup
5. Test Application: runs test_app.py again against the project
Post actions print a success or failure message.

## Steps
1. Installed Python and Flask inside the Jenkins container (it has no Python by default):
       docker exec -u root jenkins bash -c "apt-get update && apt-get install -y python3 python3-flask"
2. Created the app, test and Jenkinsfile in devops-sample-code and pushed them to the main branch.
3. In Jenkins, created a Pipeline job named Python-MultiStage-Pipeline:
   - Definition: Pipeline script from SCM
   - SCM: Git, repository URL above, branch */main, script path Jenkinsfile
4. Clicked Build Now and checked the Stage View and Console Output.

## Changes from the lab sheet
- The Build stage prints the Python and Flask versions, because the original stage only printed a message
- The Deploy stage uses a double-quoted shell block so the workspace path is substituted
- Python and Flask were installed in the Jenkins container with apt-get, as described in the lab's error-handling step

## Screenshots
- screenshots/01-stage-view.png (all pipeline stages green)
- screenshots/02-console-output.png (console output ending in Finished: SUCCESS)

## Result
All stages passed and the console output ended with "Pipeline completed successfully!" and "Finished: SUCCESS".
