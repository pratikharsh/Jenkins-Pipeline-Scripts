# Jenkins Pipeline to Start AWS EC2 Instance

## Overview
This Jenkins pipeline script is designed to start a single or multiple AWS EC2 instances. It utilizes AWS CLI commands and Jenkins pipeline syntax to automate the process. The pipeline is defined using the Declarative Pipeline syntax, making it easy to understand and maintain.

## Prerequisites
Before using this pipeline script, ensure the following prerequisites are met:

- Jenkins installed and configured.
- AWS CLI installed on the Jenkins server.
- AWS credentials (access key and secret access key) added to Jenkins credentials as 'aws_credentials'.
- Target EC2 instance ID and AWS region specified in the script.
## Pipeline Structure
The pipeline is structured as follows:

```
pipeline {
    agent any

    environment {
        INSTANCE_ID = 'i-0bb52a97242f58696'
        AWS_REGION = 'eu-north-1'
    }

    stages {
        stage('Start EC2 Instance') {
            steps {
                script {
                    withCredentials([
                        [$class: 'AmazonWebServicesCredentialsBinding', 
                         accessKeyVariable: 'AWS_ACCESS_KEY_ID', 
                         secretKeyVariable: 'AWS_SECRET_ACCESS_KEY', 
                         credentialsId: 'aws_credentials']
                    ]) {
                        sh "aws ec2 start-instances --instance-ids ${INSTANCE_ID} --region ${AWS_REGION}"
                    }
                }
            }
        }
    }
}
```
## Instructions
1. **Configure AWS Credentials in Jenkins:**
    - Add AWS access key and secret access key to Jenkins credentials with the ID 'aws_credentials'.
2. **Edit Pipeline Environment:**
    - Modify the `**INSTANCE_ID**`  and `**AWS_REGION**`  variables in the `**environment**`  block to match your EC2 instance and AWS region.
3. **Run Pipeline:**
    - Trigger the pipeline manually or set up a trigger mechanism (e.g., webhook) for automatic execution.
4. **View Execution:**
    - Monitor the pipeline execution in the Jenkins dashboard to ensure the EC2 instance starts successfully.




