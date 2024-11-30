# Jenkins
Jenkins is a self-contained, open source automation server which can be used to automate all sorts of tasks related to building, testing, and delivering or deploying software.
## What is Jenkins Pipeline?
- Jenkins Pipeline is a suite of plugins which supports implementing and integrating continuous delivery pipelines into Jenkins.
- A continuous delivery (CD) pipeline is an automated expression of your process for getting software from version control right through to your users and customers. Every change to your software (committed in source control) goes through a complex process on its way to being released. This process involves building the software in a reliable and repeatable manner, as well as progressing the built software (called a "build") through multiple stages of testing and deployment.
---
### Create a pipeline project to print "hello devops" by writing pipeline script
```
pipeline {
    agent any
    
    stages {
        stage('Print Message') {
            steps {
                echo 'hello devops'
            }
        }
    }
}
```
#### Explanation of the Script
- `pipeline`: Declares the start of a pipeline block.
- `agent any`: Specifies that the pipeline can run on any available Jenkins agent.
- `stages`: Contains one or more stages of the pipeline.
- `stage`('Print Message'): A single stage named Print Message.
- `steps`: Contains the actions to be executed in the stage.
- `echo`: Prints Hello DevOps to the Jenkins console log.
### How to Run a Script:
- Save the pipeline.
- Click Build Now.
- Check the console output to see the message Hello DevOps.
---
### Create a pipeline project to print Maven Life Cycle by writing pipeline script.
```
pipeline {
    agent any // Runs on any available agent

    stages {
        stage('Validate') {
            steps {
                echo '1. Validate: Validates the project is correct and all necessary information is available.'
            }
        }

        stage('Compile') {
            steps {
                echo '2. Compile: Compiles the source code of the project.'
            }
        }

        stage('Test') {
            steps {
                echo '3. Test: Runs the unit tests using a suitable testing framework.'
            }
        }

        stage('Package') {
            steps {
                echo '4. Package: Packages the compiled code into a distributable format (e.g., JAR, WAR).'
            }
        }

        stage('Verify') {
            steps {
                echo '5. Verify: Verifies the package meets quality criteria.'
            }
        }

        stage('Install') {
            steps {
                echo '6. Install: Installs the package into the local repository for use as a dependency in other projects.'
            }
        }

        stage('Deploy') {
            steps {
                echo '7. Deploy: Deploys the package to a remote repository.'
            }
        }
    }

    post {
        always {
            echo 'Maven Lifecycle pipeline execution completed!'
        }
    }
}
```
---
#### Explanation of the Script:
- **Pipeline Stages**:
  - Each stage corresponds to one phase of the Maven lifecycle.
  - A descriptive `echo` command explains what happens during that phase.
- **Post Actions:**
  - A `post` block with an `always` condition ensures a final message is displayed once the pipeline finishes.

### Create a pipeline project to pull source code to Jenkins server
```
pipeline {
    agent any // Runs on any available agent

    environment {
        GIT_REPO_URL = 'https://github.com/Mallick17/laravel.git' // Replace with your Git repository URL
        GIT_BRANCH = 'master' // Replace with the branch you want to pull
    }

    stages {
        stage('Pull Source Code') {
            steps {
                echo 'Cloning the source code from the Git repository...'

                // Pull the source code using Git
                git branch: "${GIT_BRANCH}", url: "${GIT_REPO_URL}"
            }
        }
    }

    post {
        success {
            echo 'Source code pulled successfully!'
        }
        failure {
            echo 'Failed to pull the source code.'
        }
    }
}
```
#### Explanation of the Script
- `pipeline`: Defines the structure of the Jenkins pipeline.
- `agent any`: Specifies that the pipeline can run on any available Jenkins agent (worker node).
- `environment`: Declares environment variables that are accessible throughout the pipeline.
- `GIT_REPO_URL`: Contains the URL of the Git repository to clone.
- `GIT_BRANCH`: Specifies the branch to pull (e.g., master).
- `stage`('Pull Source Code'): Represents the step where the source code is pulled from the repository.
- `echo`: Prints a message to the Jenkins console log to indicate progress.
- `git`: Built-in Jenkins function to clone a Git repository.
	- `branch`: Specifies the branch to clone (e.g., master)
	- `url`: The Git repository URL (e.g., https://github.com/Mallick17/laravel.git).
	- `success`: Actions to execute if the pipeline completes successfully (e.g., log a success message).
	- `failure`: Actions to execute if the pipeline fails (e.g., log a failure message).
#### How the Pipeline Works
- 1-`Setup`: Jenkins initializes the pipeline and sets the environment variables.
- 2-`Agent`: The pipeline runs on any available agent.
- 3-`Stage Execution`:
	- `Prints a message`: "Cloning the source code from the Git repository..."
	- Clones the repository specified in GIT_REPO_URL and the branch specified in GIT_BRANCH.
- 4-`Post-Execution`:
	- If successful, it prints: "Source code pulled successfully!"
	- If it fails, it prints: "Failed to pull the source code."
 ---
 ### Create pipeline project to pull source code to Jenkins server and to perform build operation by using maven tool & write a pipeline script.
 ```
pipeline {
    agent any // Use any available Jenkins agent

    environment {
        GIT_REPO_URL = 'https://github.com/Mallick17/manish_webapp.git' // Git repository URL
        GIT_BRANCH = 'master' // Branch to pull
    }

    stages {
        stage('Pull Source Code') {
            steps {
                echo 'Pulling source code from Git...'
                git branch: "${GIT_BRANCH}", url: "${GIT_REPO_URL}" // Clone the repository
            }
        }

        stage('Clean') {
            steps {
                echo 'Cleaning the project...'
                sh 'mvn clean' // Clean up the previous build artifacts
            }
        }

        stage('Validate') {
            steps {
                echo 'Validating the project...'
                sh 'mvn validate' // Validate the project structure and configuration
            }
        }

        stage('Compile') {
            steps {
                echo 'Compiling the project...'
                sh 'mvn compile' // Compile the source code
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh 'mvn test' // Run the unit tests
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging the project...'
                sh 'mvn package' // Package the compiled code into a JAR/WAR
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}
```
#### Explanation:
- **Pull Source Code**: Clones the Git repository.
- **Clean**: Runs `mvn clean` to remove any previous build artifacts.
- **Validate**: Runs `mvn validate` to ensure the project structure is valid.
- **Compile**: Runs `mvn compile` to compile the source code.
- **Test**: Runs `mvn test` to execute unit tests.
- **Package**: Runs `mvn package` to package the compiled code into a deployable artifact.
---
