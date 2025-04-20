pipeline {
  agent any

  environment {
    MAVEN_HOME = tool 'Maven 3.9.3'
  }

  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/enami04/jhipster-sampla-app.git'
      }
    }

    stage('Build') {
      steps {
        sh './mvnw clean compile'
      }
    }

    stage('Test') {
      steps {
        sh './mvnw test'
      }
    }

    stage('Jacoco Report') {
      steps {
        sh './mvnw jacoco:report'
      }
    }

    stage('Package') {
      steps {
        sh './mvnw package'
      }
    }

    stage('Archive Artifact') {
      steps {
        archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
      }
    }

    stage('Deploy (Optionnel)') {
      when {
        branch 'main'
      }
      steps {
        echo 'Déploiement simulé (Docker/Kubernetes possible ici)'
      }
    }
  }

  post {
    failure {
      mail to: 'elabjani.yassmine@gmail.com',
           subject: " Échec du build Jenkins",
           body: "Le build de ${env.JOB_NAME} a échoué à l’étape ${env.STAGE_NAME}"
    }
  }
}
