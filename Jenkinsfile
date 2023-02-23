pipeline {
  agent any
  stages {
    stage('git checkout') {
      steps {
        git(url: 'https://github.com/gordonc64/demo-jenkins-pipeline', branch: 'master', changelog: true, poll: true)
      }
    }

    stage('ready application') {
      parallel {
        stage('compile') {
          steps {
            sh './gradlew classes'
          }
        }

        stage('test') {
          steps {
            sh './gradlew test'
          }
        }

      }
    }

    stage('run checks') {
      parallel {
        stage('checkstyle') {
          steps {
            sh './gradlew check'
          }
        }

        stage('security checks') {
          steps {
            sh './gradlew --stacktrace --scan --warning-mode all dependencyCheckAnalyze'
          }
        }

      }
    }
    
    //stage('Staging') {
    //  steps {
    //    echo 'Build Docker image'
    //    sh './gradlew dockerBuildImage'
    //  }
    //}

  }
  
  //tools {
  //  gradle 'gradle6.5'
  //}
  
  post {
      always {
          dependencyCheckPublisher pattern: 'build/reports/dependency-check-report.xml'
      }
  }
}
