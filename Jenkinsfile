pipeline {
  agent any
  tools {
    // Install the Maven version configured as "M3" and add it to the path.
    maven "Maven3"
  }
  environment {
    SONAR_TOKEN = credentials('dc0c6c7c-c8ec-4926-bcb2-75b35820b9ad') // Add in Jenkins credentials
  }
  environment {
    NEXUS_USER = credentials('nexus-creds')
  }
  stages {
    stage('Checkout') {
      steps {
        // Get some code from a GitHub repository
        git 'https://github.com/jglick/simple-maven-project-with-tests.git'
      }
      stages('Build') {
        steps {
          sh 'mvn clean package'
        }
      }
      stages('SonarQube Analysis') {
        steps {
          withSonarQubeEnv('SonarQube') {
            sh 'mvn sonar:sonar'
          }
        }
      }
      stage('Publish to Nexus') {
        steps {
          sh ""
          "
          mvn deploy\
            -
            Dnexus.username = $ {
              NEXUS_USER_USR
            }\ -
            Dnexus.password = $ {
              NEXUS_USER_PSW
            }
          ""
          "
        }
      }
    }
  }
}
