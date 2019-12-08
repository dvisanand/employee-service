pipeline {
  agent any
  tools { 
        maven 'Maven'
        jdk 'Java'
  }
  stages {
    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace... */
      steps {
        checkout scm
      }
    }
    stage('Build') {
      steps {
        sh 'mvn -B -DskipTests clean package'
        sh 'echo $USER'
        sh 'echo whoami'
      }
    }
    stage('Docker Build') {
      steps {
        sh '/usr/bin/docker build -t employee-service .'
      }
    }   
    stage('push image to ECR'){
      steps {
		withDockerRegistry(credentialsId: 'ecr:ap-south-1:aws-creds', url: 'http://118463809662.dkr.ecr.ap-south-1.amazonaws.com/employee-repo') 
	       {
          	sh 'docker tag employee-service:latest 118463809662.dkr.ecr.us-east-1.amazonaws.com/employee-repo:latest'
          	sh 'docker push 118463809662.dkr.ecr.ap-south-1.amazonaws.com/employee-repo:latest'
      	       }
      }
    }
    /*stage('deploy to EKS') {
      steps {
	 node('eks-master-node'){
	     checkout scm
		 withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-creds', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
	          sh '/usr/local/bin/kubectl apply -f deployment.yaml' 
                  sh '/usr/local/bin/kubectl apply -f service.yaml' 
		 }
	 }
      }
    }*/
  }
}
