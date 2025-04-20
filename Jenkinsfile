pipeline {
  agent any
  stages {
    stage('ScrutationSCM') {
      parallel {
        stage('ScrutationSCM') {
          steps {
            echo ' Scrutation du dépôt Git'
            echo " Branche active : ${env.BRANCH_NAME}"
          }
        }

        stage('') {
          steps {
            sh '''./mvnw clean compile
'''
          }
        }

      }
    }

    stage('Compilation avec Maven') {
      parallel {
        stage('Compilation avec Maven') {
          steps {
            echo ' Compilation du code source'
            sh './mvnw clean compile'
          }
        }

        stage('') {
          steps {
            sh '''mvn clean compile
'''
          }
        }

      }
    }

    stage('JUnit') {
      parallel {
        stage('JUnit') {
          steps {
            echo ' Tests unitaires JUnit'
            sh './mvnw test'
            junit '**/target/surefire-reports/*.xml'
          }
        }

        stage('') {
          steps {
            sh '''./mvnw test
'''
          }
        }

      }
    }

    stage('Jacoco Report') {
      parallel {
        stage('Jacoco Report') {
          steps {
            echo ' Rapport de couverture Jacoco'
            sh './mvnw jacoco:report'
          }
        }

        stage('') {
          steps {
            sh '''./mvnw jacoco:report
'''
          }
        }

      }
    }

    stage('Intégration des PR') {
      parallel {
        stage('Intégration des PR') {
          steps {
            echo ' Pull Requests : merges depuis feature/* vers dev'
            sh 'git log --oneline -5'
          }
        }

        stage('') {
          steps {
            sh '''echo " Historique des derniers merges :"
git log --oneline --merges -n 5
'''
          }
        }

      }
    }

    stage('Packaging') {
      steps {
        echo ' Création du package .jar'
        sh './mvnw package'
      }
    }

    stage('Archive Artifact') {
      steps {
        echo ' Archivage de l’artefact'
        archiveArtifacts(artifacts: 'target/*.jar', fingerprint: true)
      }
    }

    stage('Déploiement conditionnel') {
      when {
        branch 'dev'
      }
      steps {
        echo ' Simulation de déploiement sur branche \'dev\''
        echo ' (Docker ou Kubernetes possible ici)'
      }
    }

  }
  environment {
    MAVEN_HOME = 'Maven 3.9.3'
  }
  post {
    success {
      echo " Pipeline exécuté avec succès sur ${env.BRANCH_NAME}"
    }

    failure {
      mail(to: 'elabjani.yassmine@gmail.com', subject: " Échec du build : ${env.JOB_NAME} [${env.BUILD_NUMBER}]", body: """Le pipeline a échoué à l’étape : ${env.STAGE_NAME}
                           Branche : ${env.BRANCH_NAME}
                           Voir les logs ici : ${env.BUILD_URL}""")
    }

  }
  options {
    skipStagesAfterUnstable()
    timestamps()
  }
}