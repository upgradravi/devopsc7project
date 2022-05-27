@Library('github.com/releaseworks/jenkinslib') _

pipeline {
    agent any
    environment {
        registry = "624128028216.dkr.ecr.us-east-1.amazonaws.com/devopsc7"
    }

    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/ravi02518/devopsc7assignment.git']]])
            }
        }

    // Building Docker images
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry
        }
      }
    }

    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
        steps{
            script {
                sh 'docker login -u AWS -p $(aws ecr get-login-password --region us-east-1) 624128028216.dkr.ecr.us-east-1.amazonaws.com/devopsc7 '
                sh 'docker push 624128028216.dkr.ecr.us-east-1.amazonaws.com/devopsc7'
            }
        }
    }

    stage('Docker Run') {
     steps{
         script {
             sshagent(credentials : ['aws-credentials']){

                sh 'ssh -o StrictHostKeyChecking=no -i Chenna_Course_Assignment.pem ubuntu@10.0.2.136'

             }
                //sh 'ssh -i /login/Chenna_Course_Assignment.pem ubuntu@10.0.2.136'
                sh 'docker run -d -p 8081:8080 --rm --name devopsc7 624128028216.dkr.ecr.us-east-1.amazonaws.com/devopsc7'
            }
      }
    }
    }
}
