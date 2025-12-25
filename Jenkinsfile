pipeline {
  agent any

  tools {
    jdk 'java17'
    maven 'maven3'
  }

  stages {

    stage('Checkout') {
      steps {
        echo 'Code already present from SCM'
      }
    }

    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv('sonarqube-server') {
          sh 'mvn sonar:sonar'
        }
      }
    }

    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('Upload to Nexus') {
      steps {
        nexusArtifactUploader(
          nexusVersion: 'nexus3',
          protocol: 'http',
          nexusUrl: '172.31.39.56 :8081',
          repository: 'maven-releases',
          credentialsId: 'nexus-creds',
          groupId: 'com.devops.demo',
          version: '1.0',
          artifacts: [
            [artifactId: 'mini-ci-app',
             file: 'target/mini-ci-app-1.0.jar',
             type: 'jar']
          ]
        )
      }
    }
  }
}
