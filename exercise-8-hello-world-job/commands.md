# Exercise 8: Creating a "Hello World" Jenkins Job

## Objective
Create a script, push it to GitHub, and run it from a Jenkins Freestyle job.

## Repository used
https://github.com/jhansinis23-hue/devops-sample-code (public)

## hello-world.sh
    #!/bin/bash
    echo "Hello, Jenkins!"

## Steps
1. Created the public GitHub repository devops-sample-code.
2. Created hello-world.sh locally, committed it and pushed it to the main branch:
       git init -b main
       git add hello-world.sh
       git commit -m "Add hello-world.sh"
       git remote add origin https://github.com/jhansinis23-hue/devops-sample-code.git
       git push -u origin main
3. Started Jenkins (Docker, available at http://localhost:9090).
4. Created a Freestyle project named HelloWorld:
   - Source Code Management: Git, repository URL above, branch specifier */main
   - Build step: Execute shell with "sh hello-world.sh"
5. Clicked Build Now and opened the Console Output.

## Notes
- The branch specifier was changed from the default */master to */main to match the repository.
- The first build failed with AccessDeniedException on /var/jenkins_home/workspace because the volume had been used by a root container in Exercise 6. It was fixed by running:
      docker exec -u root jenkins chown -R jenkins:jenkins /var/jenkins_home
  After that the build succeeded.

## Screenshots
- screenshots/01-github-repo.png (hello-world.sh in the GitHub repository)
- screenshots/02-jenkins-console.png (Jenkins console output)

## Result
The console output showed "Hello, Jenkins!" and "Finished: SUCCESS".
