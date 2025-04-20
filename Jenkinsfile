pipeline {
  agent any

  environment {
    MAVEN_HOME = tool 'Maven 3.9.3'
  }

  options {
    skipStagesAfterUnstable()
    timestamps()
  }

  stages {

    stage('ScrutationSCM') {
      steps {
        echo " Scrutation du dépôt Git pour surveiller les changements"
        echo " Branche active : ${env.BRANCH_NAME}"
      }
    }

    stage('Build') {
      parallel{
        stage('Compilation avec Maven'){
      steps {
        echo " Compilation du code source avec Maven"
        sh './mvnw clean compile'
      }
        }
    }
    }

    stage('Test') {
      parallel{
        stage('JUnit'){
      steps {
        echo " Exécution des tests unitaires JUnit (rédigés par l'équipe)"
        sh './mvnw test'
        junit '**/target/surefire-reports/*.xml'
      }
      }
    }
    }

    stage('Jacoco Report') {
      steps {
        echo " Rapport de couverture de tests généré avec Jacoco"
        sh './mvnw jacoco:report'
      }
    }

    stage('Code Review et Historique') {
      parallel{
        stage('Intégration des PR'){
      steps {
        echo " Intégration des Pull Requests depuis les branches :"
        echo "→ feature/modif1, modif2, modif3, modif4"
        echo "→ Toutes mergées dans 'dev' via GitHub"
        sh 'git log --oneline -5'
      }
    }
    }
    }

    stage('Packaging') {
      steps {
        echo " Création du package .jar"
        sh './mvnw package'
      }
    }

    stage('Archive Artifact') {
      steps {
        echo " Archivage de l’artefact final (.jar)"
        archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
      }
    }

    stage('Analyse statique du code') {
      parallel {
        stage('Checkstyle') {
          steps {
            echo " Analyse Checkstyle"
            sh './mvnw checkstyle:checkstyle'
          }
        }
        stage('PMD') {
          steps {
            echo " Analyse PMD"
            sh './mvnw pmd:pmd'
          }
        }
        stage('FindBugs (optionnel)') {
          steps {
            echo " FindBugs est obsolète, peut être remplacé par SpotBugs"
            echo " Étape laissée vide ici"
          }
        }
      }
    }

    stage('Déploiement conditionnel') {
      when {
        branch 'dev'
      }
      steps {
        echo " Simulation de déploiement sur branche 'dev'"
        echo " Docker ou Kubernetes possible à cette étape"
      }
    }
  }

  post {
    success {
      echo " Pipeline exécuté avec succès sur la branche ${env.BRANCH_NAME}"
    }

    failure {
      mail to: 'elabjani.yassmine@gmail.com',
           subject: " Échec du build : ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
           body: """Le pipeline a échoué à l’étape : ${env.STAGE_NAME}
                     Branche : ${env.BRANCH_NAME}
                     Voir les logs ici : ${env.BUILD_URL}"""
    }
  }
}

