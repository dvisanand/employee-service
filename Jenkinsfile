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
	      withDockerRegistry([credentialsId: 'docker-creds', url: "https://index.docker.io/v1/"]) {
          sh '/usr/bin/docker tag employee-service dvisanand/employee-service:latest'
          sh '/usr/bin/docker push dvisanand/employee-service:latest'
      }
	}
    }
    stage('deploy to EKS') {
      steps {
         sh '/home/ec2-user/bin/kubectl apply -f deployment.yaml' 
         sh '/home/ec2-user/bin/kubectl apply -f service.yaml' 
      }
    }
  }
}
