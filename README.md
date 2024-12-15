# JENKINS - CI/CD - PIPELINE

👋 Welcome to the CI/CD world!

# What is CI/CD?

CI- Continuos Integration

CD - Continuos Deployment

CD - Continuos Delivery

### Continuous Integration:

> Continuous Integration (CI) is the practice where developers regularly merge their code changes into a shared repository using a version control system (VCS) like GitHub. The primary goal is to detect bugs early and ensure seamless code integration.
> 

### Continuos Deployment:

> Continuous Deployment (CD) refers to the automated process of deploying code changes directly to various environments, such as development, staging, and production after successful integration and testing , Jenkins handles the process to make it smooth and fast.
> 

### Continuos Delivery:

> Continuous Delivery (CD) means the application is always ready to be deployed to the production environment. The code is tested and prepared, so it can be released anytime.
> 

### How It Works:

When developers push their code to a shared repository, a CI/CD pipeline automatically performs a series of actions.

`🚀 Developer writes code → 🛠  Pushes to **GitHub** → 🤖 CI/CD`

Several tools can create CI/CD pipelines, including Jenkins, GitLab, and Travis CI. Jenkins, being the most widely used, will be our focus.

Jenkins monitors the GitHub repository continuously. When it detects changes, it automatically triggers a pipeline that fetches code, installs dependencies, runs tests, builds the application, and deploys it. This workflow is standard across most organizations.

`🚀 Developer writes code → 🛠 Pushes changes to **GitHub** → 🤖 **Jenkins (CI/CD Tool)**` 

**Let's explore how this works with a React application using Jenkins, going through it step by step.**

1. Imagine you're working at an organization with a React-based e-commerce project. Your manager assigns you to implement new functionality requested by the client.
2. After updating the functionality and pushing the code to GitHub, Jenkins—which continuously monitors for GitHub changes—automatically initiates its pipeline. When code changes are detected, Jenkins executes a series of staged actions: **`fetching the code, install the dependencies, run the tests, builds , deploys it *`.***While these are the standard stages most organizations follow, the number and type of stages can be customized based on specific requirements.
3. Usually Jenkins deploys the application to  three environments 
- Dev Environment
    
    Development environment where the application undergoes testing. If all tests pass successfully, the application is deployed to the staging environment.
    
- Staging Environment
    
    This is a pre-production environment where the application is deployed for final testing and validation before moving to production. It serves as the last verification stage before the production release.
    
- Production Environment
    
    This is the live environment where end users or clients interact with and access the application
    

### Workflow Overview:

🚀 **App** → 🛠 **GitHub** → 🤖 **Jenkins** → 🖥 **Dev** → 🧪 **Staging** → 🌐 **Production**

### Key Steps in the Workflow:

1. **App:** User interacts with the application.
2. **GitHub:** Source code is managed in repositories.
3. **Jenkins:** CI/CD pipeline automates builds.
4. **Dev:** Development environment testing.
5. **Staging:** Pre-production validation.
6. **Production:** Live environment for users.

## How to do CI/CD in practical by Jenkins tool:

### Setting up Jenkins on AWS:

Let’s see how actually it done in practical in step by step manner,

- Step 1:  Create EC2 instance
    - [Go to AWS Management Console](https://aws.amazon.com/console/)  and create a account on AWS, select root user and sign in to AWS
    - After sign in to AWS, it shows a dashboard where you can see the services like `EC2, Cloud Watch, S3` etc..,
    - Click on `EC2`
    - On top right corner you see `Launch Instance`  click on it
    - After that you see `Names and tags`  under that it has `Name`  write a name of your instane that you want to create
    - After that you see **`Application and OS Images (Amazon Machine Image)**`   under that you see `Recents and Quick Start`  select Ubuntu ( in my case i select ubuntu, you can select according to your choice)
    - Under that you have **`Key pair (login)**`    click **`Create new key pair`**  and enter name as you want to name the key according to your choice and in below you see **`Private key file format`**  by default it has **`.pem`**  keep it as it before and next Click `Create Key Pair`  and `Launch Instance`
    - Click instances after few seconds you will see the Instance is running , if you not see you will `All State` toggle button click on it and then click `Running` .
    - After click the instance you will see a dashboard in which you will see all the details regarding to instances like `Public IP address and Private IP address`
- Step 2:  SSH into the EC2 Instance
    - Go to your terminal and run the below commands
    - `ssh -i /Users/username/Downloads/key-pair.pem ubuntu@instance-public-ip-address` ( i use mac, so in mac it looks like this, you have apple id in which you give user name, that will be username, key-pair.pem will be in step 1 we create a key pair write name of that file .pem is a extension of that, i select ubuntu machine and paste your public ip address, it will look as, `ssh -i /Users/abc/Downloads/jenkins.pem ubuntu@100.23.45.6`
    - after that it asks for yes/no, type yes, and then it shows permission denied, to grant permission you will run this `chmod 600 /Users/username/Downloads/key-pair.pem`  and after that `ssh -i /Users/username/Downloads/key-pair.pem ubuntu@instance-public-ip-address`   now you will see the the ip address like `ubuntu@172.870.990.000`
    
- Step 3:  Install Jenkins on the EC2 Instance
    
    **Install Java** (Jenkins requires Java to run) 
    
    ```jsx
    sudo apt update
    sudo apt install openjdk-11-jdk -y  # For Ubuntu
    ```
    
    Verify Java Installed
    
    ```jsx
    sudo apt update
    sudo apt install openjdk-11-jdk -y  # For Ubuntu
    ```
    
    Install Jenkins
    
    ```jsx
    curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
      /usr/share/keyrings/jenkins-keyring.asc > /dev/null
    echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
      https://pkg.jenkins.io/debian binary/ | sudo tee \
      /etc/apt/sources.list.d/jenkins.list > /dev/null
    sudo apt-get update
    sudo apt-get install jenkins
    ```
    
    Verify Jenkins running on which port, usually it runs on port 8080
    
    ```jsx
    ps -ef | grep jenkins
    
    # you will see jenkin 8080 after running above command
    ```
    
- Step 4: Create Security group on EC2 Instance and Login to Jenkins
    - Click on EC2 Instance and after that you will see all the information related to instance, under that you  will `security groups tab`, click on it click on the security group you will navigate to the tab where you see `Add inbound traffic rule`, click `Add rule`  and inbound traffic rule to only allow `custom TCP` port `8080`
    - After that copy `EC2 Public IP Address`   and go to browser and paste the public ip address  like below, always use http not https it looks like `http:public-ip-address:8080`
    - After that you will see on browser like `Getting started`  and `Unlock Jenkins`
    - After that you will see like this `/var/lib/jenkins/secrets/initialAdminPassword`  copy that and run on terminal
    - Like `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`  you will see a password after running this copy it and paste it in a `Administrator password` tab
- Step 5: Install the Plugins
    - click on Install suggested plugins and Wait for the Jenkins to Install suggested plugins
    - After that Create First Admin User and remember the name and password that your type in
    - After that you will see  `Jenkins is Ready`  click `start using jenkins`
    
- Step 6: Install the Docker Pipeline plugin in Jenkins
    - Go to Manage Jenkins > Manage Plugins
    - In the Available tab, search for "Docker Pipeline".
    - Select the plugin and click the Install button.
    - Restart Jenkins after the plugin is installed.
- Step 7:  Docker Slave Configuration
    - In terminal run this commands
    
    ```jsx
    sudo apt update
    sudo apt install docker.io
    
    sudo su - 
    usermod -aG docker jenkins
    usermod -aG docker ubuntu
    systemctl restart docker       // output: root@ip-172-54-67-8-9-9
    
    su - jenkins // output: jenkins@ip-172-54-67-8-9
    docker run hello-world // output: permission denied
    logout // output: root@ip-172-54-67-8-9-9
    
    usermod -aG docker jenkins
    su - jenkins // output: jenkins@ip-172-54-67-8-9
    docker run hello-world // run succesfully
    
    ```
    

### How to create pipeline in Jenkins :

Let’s see how to create in step by step manner.

- On Jenkins dashboard Click + `New Item`
- It shows the options like `free style project, pipeline, folder etc..,`
- Click on pipeline and give the job name the pipeline that you want to give
- After that it shows Description in which you can give information about the pipeline, now scroll to bottom you will see `pipeline with script, pipeline scm`   this is your choice if we want to define stages on jenkins tool it self then click `pipeline with script`   and write the stages
- Instead of writing the stages on tool, you can select `pipeline scm`
- If you select `Pipeline scm`  just below you will see a dropdown which contains `none and Git` select `Git`
- After selecting `Git` it shows `repository URL`, branch `master or main` initially it has `master`, below path `Jenkinsfile`
- Now, if you select `Pipeline scm`  you have to write stages on your project directory like create Jenkinsfile and write stages according to your project requirements, here stages nothing but set of actions that we see above
- After creating a Jenkinsfile and define stages on it on project directory, paste the repo URL on `repository URL`
- Select branch if your branch is master write master if its main then write main
- After that if you create a seperate folder for jenkins file then click on  folder on github and copy the folder name from the url and add it before the `Jenkinsfile` in the path below  like `my-first-ci-cd-pipeline/Jenkinsfile`  it should be look like this
- Click **Save** to apply the changes.
- Click **Build Now** in the job dashboard to manually start the pipeline.
- Under you will build is creating, click on `…` three dots you will see the options and click on `console output`  you will see how it fetches and executes, if all good you will see `Finish Success`  if any thing fails you will see `Failed`
- After if you go to dashboard you will see the pipelines that you have created on jenkins and you can all the details there like time, status etc..,

This is how we can create a pipeline on Jenkins

### How to write stages in jenkins file:

Above we have seen how to create the pipeline, now we will see how to write stages in jenkinsfile 

The basic syntax for writing stages is 

```jsx
Declarative Jenkins Pipeline with SCM (GitHub): 
pipeline {
    agent any  // Use any available agent

    environment {
        REPO_URL = 'https://github.com/yourusername/your-repository.git'  // Replace with your GitHub repo URL
        BRANCH_NAME = 'main'  // The branch to checkout (e.g., 'main')
    }

    stages {
        stage('Checkout or fetches code') {
            steps {
                // Checkout code from GitHub repository
                git url: REPO_URL, branch: BRANCH_NAME
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install project dependencies
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                // Build the React app
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                // Run tests (optional)
                sh 'npm test'
            }
        }

        stage('Deploy') {
            steps {
                // Deploy the app (replace with your actual deployment step)
                echo 'Deploying the app...'
            }
        }
    }

    post {
        always {
            // Actions that always run after the pipeline
            echo 'Pipeline execution completed.'
        }
        success {
            // Actions on success
            echo 'Pipeline executed successfully!'
        }
        failure {
            // Actions on failure
            echo 'Pipeline failed!'
        }
    }
}

   
If we use agent as docker then,

pipeline {
    agent {
        docker { image 'node:16-alpine' }
    }
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
    }
    post {
        failure {
            echo 'Build failed. Please check the console output for errors.'
        }
    }
}

The stages may increase or decrease but it should be like above, the scripting might chnage
 for project to project but syntax should be like this, these are just examples.
        

```

### Conclusion

Jenkins is one of the best and most useful tools in DevOps pipelines ⚙️. By automating tasks like building 🔨, testing 🧪, and deploying 🚀, it helps teams deliver software faster, more reliably, and with fewer mistakes. Learning Jenkins will improve your CI/CD skills 💡 and make your development process smoother 🌱.

### What to do next

If you’re new to Jenkins, start by trying simple pipelines 🛠️. As you get more comfortable, try using Jenkins with other tools like Docker 🐳, Kubernetes ☸️, or AWS ☁️. Jenkins also has many helpful plugins 🔌 to add more features.
