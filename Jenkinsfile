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
          sshPublisher(publishers: [sshPublisherDesc(configName: 'Docker-host', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'docker rmi pcontact:v.${BUILD_NUMBER}; docker build -t pcontact:v.${BUILD_NUMBER} .; docker tag pcontact:v.${BUILD_NUMBER} kserge2001/pcontact:v.${BUILD_NUMBER}; docker push kserge2001/pcontact:v.${BUILD_NUMBER};', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: 'webapp/target', sourceFiles: '**/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
     
      }
    }
  }
}
