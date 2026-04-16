pipeline {
  agent none
  stages {
    stage('Build') {
      agent {
        docker {
          image 'maven:3.9.9-eclipse-temurin-17-alpine'
        }

      }
      steps {
        echo 'Compiling sysfoo app..'
        sh 'mvn compile'
      }
    }

    stage('Test') {
      agent {
        docker {
          image 'maven:3.9.9-eclipse-temurin-17-alpine'
        }

      }
      steps {
        echo 'Running Unit Testing..'
        sh 'mvn clean test'
      }
    }

    stage('Package') {
      agent {
        docker {
          image 'maven:3.9.9-eclipse-temurin-17-alpine'
        }

      }
      steps {
        echo 'Creating package for the app....'
        sh '''#!/bin/bash
GIT_SHORT_COMMIT=$(echo $GIT_COMMIT | cut -c 1-7)
mvn versions:set -DnewVersion="$GIT_SHORT_COMMIT"
mvn versions:commit'''
        sh 'mvn package -DskipTests'
        archiveArtifacts(artifacts: '**/target/*.jar', allowEmptyArchive: true)
      }
    }

  }
  tools {
    maven 'maven 3.9.12'
  }
  post {
    always {
      echo 'This pipeline is completed..'
    }

  }
}