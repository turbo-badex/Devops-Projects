
The following steps detail how i built a CI/CD Pipeline using Gitlab CICD on a self-managed runner.

Step 1: Setup a self-managed gitlab runner


<img width="625" alt="image" src="https://github.com/turbo-badex/Devops-Projects/assets/170211854/cb08d8b4-37d8-4cde-ac3c-0120e3f2e39f">


In your GitLab project, navigate to the settings and the CI/CD submenu. Expand the Runners section.

Select the New project runner

Select the operating system where gitlab runner will be installed. In my case, its an AWS EC2 instance.

In the Tags section, in the Tags field, enter the job tags to specify jobs the runner can run. If there are no job tags for this runner, select Run untagged.

In the Runner description field, to add a runner description that displays in GitLab, enter a runner description.

In the maximum job timeout subsection, input 1800. So the runner will not timeout even if it goes job hunting.

Select Create Runner. This should create a runner successfully. Before navigating away from that page, make sure to copy your authentication token in a safe place. 

<img width="625" alt="image" src="https://github.com/turbo-badex/Devops-Projects/assets/170211854/1e6c7e20-7c76-4410-858e-6006d938dff7">

<img width="627" alt="image" src="https://github.com/turbo-badex/Devops-Projects/assets/170211854/65c1a90d-d8f2-42ea-98c5-b8a94858d5db">







Navigate to https://docs.gitlab.com/runner/install/linux-repository.html .  
Depending on the Linux distribution your AWS EC2 instance runs on, copy the appropriate commands.

I’m running an Ubuntu-based AWS EC2 instance. Thus my command and steps are as follows

Add the official GitLab repository: curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash

<img width="622" alt="image" src="https://github.com/turbo-badex/Devops-Projects/assets/170211854/5abed754-8550-42c0-ac7b-3b3d1f6d00a2">

Install the latest version of GitLab Runner: sudo apt-get install gitlab-runner

<img width="625" alt="image" src="https://github.com/turbo-badex/Devops-Projects/assets/170211854/37bf9ebb-8a6e-419a-a056-6571064ac543">

Enable gitlab runner and check status:
 sudo systemctl enable --now gitlab-runner
sudo systemctl status gitlab-runner

<img width="625" alt="image" src="https://github.com/turbo-badex/Devops-Projects/assets/170211854/95c5f76d-1ffe-435a-aaf1-2728ea8dfbb7">







Register the runner
sudo gitlab-runner register. You’ll be prompted to enter the GitLab instance URL, registration token, and an executor. 
Enter “https://gitlab.com/”, the registration token you copied earlier, and “shell” respectively

<img width="623" alt="image" src="https://github.com/turbo-badex/Devops-Projects/assets/170211854/d3294680-2b55-4f9a-bf03-602c11b7525c">




Now visit the runner's page, you’ll see that the runner has been registered, as indicated by the green circle. 


<img width="622" alt="image" src="https://github.com/turbo-badex/Devops-Projects/assets/170211854/10df6679-4455-47be-b111-fe14467a2002">

*******IMPORTANT STEP******* - Make sure to install docker in your EC2 runner instance. You NEED docker runtime installed in the server environment to ensure a successful docker build.
https://docs.docker.com/engine/install/ubuntu/

After installing docker on your EC2 instance runner, I highly recommend you Add the GitLab CI User to the Docker Group

use the command:   sudo usermod -aG docker gitlab-runner


restart docker after adding the gitlad CI user using the command: sudo systemctl restart docker



Create a .gitlab-ci.yml file and define pipeline jobs.

<img width="822" alt="image" src="https://github.com/turbo-badex/Devops-Projects/assets/170211854/7dd2070f-70ef-4dcd-b756-c803bc12be60">


View the status of our pipeline and jobs
We can commit our changes and push the latest changes to the remote branch to trigger the start of the pipeline.


Select Build and then Pipelines to see all the jobs for that repository.


<img width="1667" alt="image" src="https://github.com/turbo-badex/Devops-Projects/assets/170211854/aa5fb834-4eb2-443e-9b02-6e5b01af97fd">

