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
          docker.withRegistry('', 'DockerRgistryID') {
          def customImage = docker.build("kserge2001/devops-pipeline:${env.BUILD_ID}")
          customImage.push()
          }
    }
}
    }
    stage('deploy'){
        steps {
          sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible-host', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook /etc/ansible/lamp.yml ;', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
     
      }
    }
  }
}
