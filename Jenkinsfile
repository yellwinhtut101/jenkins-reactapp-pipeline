pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    AN_ACCESS_KEY = credentials('ywh-credentials')
    AWS_DEFAULT_REGION    = 'us-east-1'
    IMAGE_NAME            = 'yellwinhtut/jenkins-example-react'
    IMAGE_TAG             = 'latest'
    ECR_REPO              = 'https://006961800653.public.ecr.us-east-1.amazonaws.com/v0n2c9p8/y3ll-lab'
    EC2_INSTANCE_IP       = '54.80.70.193'
    // SSH_CREDENTIALS       = credentials('your-ssh-credentials')
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
      }
    }
    stage('Test') {
      steps {
        sh 'echo "Empty"'
      }
    }
    stage('Push to Amazon ECR') {
      steps {
          sh """aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin 006961800653.public.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
          sh "docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${ECR_REPO}:${IMAGE_TAG}"
          sh "docker push ${ECR_REPO}:${IMAGE_TAG}"
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
