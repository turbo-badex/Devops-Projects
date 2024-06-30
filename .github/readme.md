the following steps and screenshots details how i created my first github workflow action


step 1: create an AWS EC2 instance. its important to select "Ubuntu image" instead of the "AMAZON LINUX" image. Leave all other settings as they are.

<img width="772" alt="image" src="https://github.com/turbo-badex/Devops-Projects/assets/170211854/d69c9573-7ffb-4833-9fd3-be902bf2eab7">


Step 2: In your GitHub account, in the repository you wish to use, visit the settings page and navigate to the runner's sub-menu page.


Step 3: Select the runner image and architecture of your EC2 instance. In my case, here I selected Linux and x64

<img width="794" alt="image" src="https://github.com/turbo-badex/Devops-Projects/assets/170211854/1a3ec0fa-3154-4938-bdb9-34a66eb1e240">


Follow the steps listed, to download and configure the runner on your self-hosted EC2 instance.

If you follow all the steps listed, you should get this message

<img width="939" alt="image" src="https://github.com/turbo-badex/Devops-Projects/assets/170211854/9e0bcf40-234d-4ce8-a799-c417708f4808">


Step 4: create a yaml file stored in .github/workflows folder in the root directory of your repository

<img width="620" alt="image" src="https://github.com/turbo-badex/Devops-Projects/assets/170211854/9e62ea37-6041-4286-a2c0-12d6cde67b10">



Step 5: Add some secrets contained in our workflow like DOCKER_USERNAME and DOCKER_KEY which are the username and password needed to login to dockerhub to push our image after building.

Go to your repository’s settings. Select secrets and variables under Actions and click on New repository secret to add the key and value.

<img width="785" alt="image" src="https://github.com/turbo-badex/Devops-Projects/assets/170211854/44f4bede-ae0d-4221-930e-02743160a429">
