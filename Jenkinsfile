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
        script {
          checkout scm
          docker.withRegistry('', 'dockerUserID') {
          def customImage = docker.build("kserge2001/devops-pipeline:${env.BUILD_ID}")
          customImage.push()
          }
    }
}
    }
    stage("SSH Steps for ansible") {
    withCredentials([usernamePassword(credentialsId: 'sshUserAcct', passwordVariable: 'password', usernameVariable: 'userName')]) {
        remote.user = userName
        remote.password = password
            writeFile file: 'test.sh', text: 'ls'
            sshCommand remote: remote, command: 'ansible-playbook /etc/ansible/deploy.yml'
    }
    }       
 }
}
