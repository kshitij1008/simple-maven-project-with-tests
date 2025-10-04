pipeline {
  agent any

  tools {
    maven "Maven3"
  }

  environment {
    SONAR_TOKEN = credentials('dc0c6c7c-c8ec-4926-bcb2-75b35820b9ad')
    NEXUS_USER = credentials('nexus-creds')
  }

  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/jglick/simple-maven-project-with-tests.git'
      }
    }

    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv('SonarQube') {
          sh 'mvn sonar:sonar'
        }
      }
    }

    stage('Publish to Nexus') {
      steps {
        writeFile file: 'jenkins-settings.xml', text: """
          <settings>
            <servers>
              <server>
                <id>nexus</id>
                <username>${NEXUS_USER_USR}</username>
                <password>${NEXUS_USER_PSW}</password>
              </server>
            </servers>
          </settings>
        """
        sh 'mvn deploy --settings jenkins-settings.xml'
      }
    }
  }
}
