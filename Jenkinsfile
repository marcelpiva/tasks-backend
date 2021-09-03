pipeline {
  agent any
  tools {
      maven 'MAVEN_DEFAULT'
      jdk 'JAVA_DEFAULT'
  }
  stages {
    stage('Build Backend') {
      steps {
        sh 'mvn clean package -DskipTests=true'
      }
    }
    stage('Unit Tests') {
      steps {
        sh 'mvn test'
      }
    }
    stage('Sonar Analysis') {
      environment {
          scannerHome = tool 'SONAR_SCANNER'
      }
      steps {
        withSonarQubeEnv('SONAR_LOCAL') {
          sh "${scannerHome}/bin/sonar-scanner -e \
              -Dsonar.projectKey=DeployBack \
              -Dsonar.host.url=http://sonar:9000 \
              -Dsonar.login=15544b409ba8e50f48677286cb229131ff1a505c \
              -Dsonar.java.binaries=target \
              -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**Application.java"
        }
      }
    }
    stage('Quality Gate') {
      steps {
        sleep(10)
        timeout(time: 1, unit: 'MINUTES') {
          waitForQualityGate abortPipeline: true
        }
      }
    }
  }
}
