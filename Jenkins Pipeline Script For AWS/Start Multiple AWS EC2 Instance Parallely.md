# PARALLELY MULTIPLE AWS  EC2 Instance Starter Jenkins Pipeline

## Overview 🚀
This Jenkins pipeline script is designed to start multiple AWS EC2 instances in parallel. It leverages Jenkins' parallel stages to efficiently initiate the start-up process for each specified instance. The script uses AWS CLI commands and requires AWS credentials to execute.

## Prerequisites 🛠️
Before using this pipeline, ensure the following prerequisites are met:

- Jenkins is installed and configured.
- AWS CLI is installed on the Jenkins server.
- AWS credentials with EC2 start permissions are set up and stored securely in Jenkins.
## Configuration 🛠️
1. **Jenkins Configuration:**
    - Make sure Jenkins is properly configured with the necessary plugins.
    - Set up the required credentials in Jenkins to access AWS. This includes AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY.
2. **Pipeline Configuration:**
    - Update the `**INSTANCE_IDS**`  variable with the comma-separated list of EC2 instance IDs you want to start.
    - Modify the `**AWS_REGION**`  variable to specify the AWS region where your instances reside.
## Pipeline Structure 📋
The pipeline script is organized as follows:

```groovy
groovyCopy codepipeline {
    agent any
    
    environment {
        INSTANCE_IDS = "i-0bb52a97242f58696,i-0b8710300cf30be25"
        AWS_REGION = 'eu-north-1'
    }
    
    stages {
        stage('Start EC2 Instances') {
            steps {
                script {
                    def instanceList = INSTANCE_IDS.tokenize(',')
                    parallel 'Start Instances': {
                        startEC2Instances(instanceList)
                    }
                }
            }
        }
    }
}

def startEC2Instances(instanceIds) {
    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY', credentialsId: 'aws_credentials']]) {
        for (String instanceId : instanceIds) {
            sh "aws ec2 start-instances --instance-ids ${instanceId} --region ${AWS_REGION}"
        }
    }
}
```
## Usage 🚀
1. Trigger the pipeline manually or set up a scheduled trigger based on your requirements.
2. Jenkins will parallelly start the specified EC2 instances using AWS CLI commands.
## Important Note ℹ️
- Ensure that the AWS credentials used in Jenkins have the necessary permissions to start EC2 instances.
---

Feel free to customize and enhance the pipeline script based on your specific needs. If you encounter any issues or have suggestions for improvements, please open an issue or submit a pull request.

Happy automating! 🤖✨

