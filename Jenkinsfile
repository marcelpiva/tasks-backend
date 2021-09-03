pipeline {
  agent any
  stages {
    stage('Build Backend') {
      steps {
        bat 'mvn clean package -DskipTests=true'
      }
    }
    stage('Unit Tests') {
      steps {
        bat 'mvn test'
      }
    }
    stage('Sonar Analysis') {
      environment {
        scannerHome = tool = 'SONAR_SCANNER'
      }
      steps {
        withSonarQuebeEnv('SONAR_LOCAL')
        bat "${scannerHome}/bin/sonnar-scanner -e \
              -Dsonar.projectKey=DeployBack \
              -Dsonar.host.url=http://sonar:9000 \
              -Dsonar.login=15544b409ba8e50f48677286cb229131ff1a505c \
              -Dsonar.java.binaries=target \
              -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**Application.java"
      }
    }
  }
}
