# CodePipeline-CI-CD
CI/CD Pipeline
<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Build a CI/CD Pipeline with AWS

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-codepipeline-updated)

**Author:** Darryl Brown  
**Email:** darrylbrown1991@gmail.com

---

![Image](http://learn.nextwork.org/sincere_vermilion_playful_beaver/uploads/aws-devops-codepipeline-updated_fbdetger)

---

## Introducing Today's Project!

In this project, I will demonstrate how to set up the foundation for my CI/CD pipeline (i.e. my source GitHub, build with CodeBuild, and deploy with CodeDeploy. I'm doing this project to learn how to put everything together an automate a CI/CD pipeline with AWS CodePipeline.

### Key tools and concepts

Services I used were CodeDeploy, EC2, IAM, CodeBuild, CodeArtifact, CodePipeline, GitHub, VScode, S3, CloudFormation. Key concepts I learnt include different stages in CI/CD pipeline, handling rollbacks in CodePipeline, amd webhooks.

### Project reflection

This project took me approximately 2 hours. The most challenging part was making sure all connect resources had proper permissions to make sure actions with other resources. It was most rewarding to successfully complete my CI/CD pipeline and automating the flow of all the resources. 

---

## Starting a CI/CD Pipeline

AWS CodePipeline is an AWS DevOps tools that helps create a workflow that automatically moves code from GitHub our source code repository all the way to CodeDeploy my deployment tool. Basically from continuous integration to continuous deployment. Using CodePipeline helps make sure my deployments are consistent, reliable and happen automatically whenever I update my code. Also with less risk for human errors! It saves me time too.

CodePipeline offers different execution modes based on how CodePipeline handles multiple runs of the same pipeline. I chose Superseded because if a new pipeline execution is triggered while another execution is already in progress, the newer execution will immediately take over and cancel the older one. This is perfect for making sure only the latest code changes are processed, which is what I want for my CI/CD pipeline! Other options include Queued mode, where executions are processed one after another. If a pipeline is already running, any new executions will wait in a queue until the current execution finishes. And Parallel mode allows multiple executions to run at the same time, completely independently of each other. This can speed up the overall processing time if I had multiple branches or code changes that can be built and deployed concurrently.

A service role gets created automatically during setup so a special type of IAM role that AWS services like CodePipeline use to perform actions on my behalf. It's like giving CodePipeline permission to access other AWS resources it needs to run my pipeline, such as S3 buckets for storing artifacts or CodeBuild for building my code.

![Image](http://learn.nextwork.org/sincere_vermilion_playful_beaver/uploads/aws-devops-codepipeline-updated_gdnhtm)

---

## CI/CD Stages

The three stages I've set up in my CI/CD pipeline are. Source stage is source code for web project and GitHub, Build stage is building the web app using CodeBuild, and Deploy stage is about deploying changes to web app using CodeDeploy.

CodePipeline organizes the three stages into a single diagram that showcases the flow from source to deploy. In each stage, you can see more details on the pipeline execution that it belongs to. And the each shortcut or link to the connected service. 

![Image](http://learn.nextwork.org/sincere_vermilion_playful_beaver/uploads/aws-devops-codepipeline-updated_fbdetger)

---

## Source Stage

In the Source stage, by specifying the master branch as the default branch I'm telling CodePipeline to monitor this branch for changes and trigger the pipeline whenever there's a commit to it.the default branch. In Git, a branch is like a parallel timeline of your project. It allows me to work on new features or bug fixes without affecting the main code base. The master branch is typically considered the main branch, representing the stable, production-ready code. 

The source stage is also where I enable webhook events, which let CodePipeline automatically start my pipeline whenever code is pushed to my specified branch in GitHub. This is what makes my pipeline truly "continuous". It reacts to code changes in real time! Webhooks are like digital notifications. By enabling webhook events, CodePipeline sets up a webhook in GitHub repository. This webhook is configured to listen for specific events, such as code pushes to the master branch. Whenever I push code to the master branch, GitHub sends a webhook a notification to CodePipeline. CodePipeline then automatically starts a new pipeline execution in response to this event. It's a seamless way to automate the CI/CD process!

![Image](http://learn.nextwork.org/sincere_vermilion_playful_beaver/uploads/aws-devops-codepipeline-updated_sergt)

---

## Build Stage

The Build stage is where my source code gets compiled and packaged into something that can be deployed. I configured Codebuild to be my build provider and to use the input artifacts that was outputted by the Source Stage. The input artifact for the build stage is a compressed code that the source stage retrieved from GitHub and compressed into a zip file.

![Image](http://learn.nextwork.org/sincere_vermilion_playful_beaver/uploads/aws-devops-codepipeline-updated_j1k2l3m4)

---

## Deploy Stage

The Deploy stage is where I set up CodeDeploy to be my deployment provider. This is the final step in my pipeline. It's responsible for taking the application artifacts (the output from the Build stage) and deploying them to the target environment, which in my case is an EC2 instance. I configured to take the build artifact from CodeBuild and the application and deployment setting that I defined in my deployment group.

![Image](http://learn.nextwork.org/sincere_vermilion_playful_beaver/uploads/aws-devops-codepipeline-updated_m4n5o6p7)

---

## Success!

Since my CI/CD pipeline gets triggered by code change in the webhook events. I tested my pipeline by adding by updating my code, I added a new line to my index.jsp file and pushed to GitHub.

The moment I pushed the code change I saw my pipeline respond immediately by trigger and new build and deploy stage. The commit message under each stage reflects the latest code change that my pipeline is using. The Source stage was the first stage to reflect latest commit and then build and deploy stage said the same thing.

Once my pipeline executed successfully, I checked Public IPv4 DNS from my EC2 instance by putting the ipv4 dns in my web browser so see the successful push through my pipeline was successful! I confirmed that I did not have to manually rebuild and changes went into production right away.

![Image](http://learn.nextwork.org/sincere_vermilion_playful_beaver/uploads/aws-devops-codepipeline-updated_e1f2g3h4)

---

## Testing the Pipeline

In a project extension, I initiated a rollback on the deploy stage of my pipeline. Automatic rollback is important for a change was deployed that passed test but is actually failing or does not look the way it should.

During the rollback, the source and build stage are unaffected because both stages come before deployment stage. I could verify this by comparing the commit messages related to each stage. The deploy stage commit message has reverted back to the commit for my first pipeline run. The other two stages are still using my latest commit messages. 

After rollback, the live web app reverted to it's original state before I did any test for changes. This means CodeDeploy rolled back successfully and instantly. 

![Image](http://learn.nextwork.org/sincere_vermilion_playful_beaver/uploads/aws-devops-codepipeline-updated_sdfgsdfgdf)

---

---
