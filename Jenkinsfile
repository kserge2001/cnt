pipeline {
  agent any
  tools {
     maven 'M2_HOME'
  }
  environment {
    registry = "kserge2001/devop-pipeline"
    registryCredential = 'dockerUserID'
  }
  stages {
    stage('Build'){
      steps {
       sh 'mvn clean'
       sh 'mvn install'
       sh 'mvn package'
      }
    }
    stage('test'){
      steps {
       echo "test step"
       sh 'mvn test'
      }
    }
    stage ('build and publish image') {
      steps {
          checkout scm
          docker.withRegistry('', 'registryCredential') {
          def customImage = docker.build("kserge2001/devops-pipeline:${env.BUILD_ID}")
          customImage.push()
    }
}
    }
      
 }
}
