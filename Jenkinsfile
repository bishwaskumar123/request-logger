pipeline {
  agent any

  tools {
    maven 'mvn'
  }

  stages {
    stage('Build App') {
      steps {
        sh 'mvn package'
      }
    }
    
    stage('Build Image') {
      steps {
      sh "docker build -t bishwaskumar123/request-logger:${env.BUILD_ID} ."
      sh "docker tag bishwaskumar123/request-logger:${env.BUILD_ID} bishwaskumar123/request-logger:latest"
      }
    }
    
  }

  post {
    always {
      archive 'target/**/*.jar'
    }
    success {
      withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
        sh "docker login -u ${USERNAME} -p ${PASSWORD}"
        sh "docker push bishwaskumar123/request-logger:${env.BUILD_ID}"
        sh "docker push bishwaskumar123/request-logger:latest"
      }
    }
  }
}
