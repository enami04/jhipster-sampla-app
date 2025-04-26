pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        git(branch: 'main', url: 'https://github.com/enami04/jhipster-sampla-app.git')
      }
    }

    stage('Build') {
      steps {
        bat './mvnw clean compile'
      }
    }

    stage('Test') {
      steps {
        bat './mvnw test'
      }
    }

    stage('JaCoCo Report') {
      steps {
        bat 'cmd /c mvnw.cmd jacoco:report'
      }
    }

    stage('Package') {
      steps {
        bat 'cmd /c mvnw.cmd package'
      }
    }

    stage('Archive Artifacts') {
      steps {
        archiveArtifacts(artifacts: 'target/*.jar', fingerprint: true)
      }
    }

  }
  environment {
    MAVEN_OPTS = '-Dmaven.test.failure.ignore=false'
  }
  post {
    failure {
      mail(to: 'elabjani.yassmine@gmail.com', subject: ' Build Échoué - jhipster-sampla-app', body: 'Le build du projet jhipster-sampla-app a échoué. Veuillez consulter Jenkins pour plus de détails.')
    }

  }
}