pipeline {
    agent any

    environment {
        MAVEN_OPTS = "-Dmaven.test.failure.ignore=false"  // Permet d'ignorer les erreurs de test
    }

    stages {
        stage('Checkout') {
            steps {
                // Cloner le dépôt depuis GitHub (utilisation de la branche main)
               git branch: 'main', url: 'https://github.com/enami04/jhipster-sampla-app.git'

            }
        }

        stage('Build') {
            steps {
                // Compiler le projet avec Maven (commande pour Windows)
                bat './mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                // Exécution des tests unitaires avec Maven
                bat './mvnw test'
            }
        }

        stage('JaCoCo Report') {
            steps {
                // Génération du rapport de couverture des tests avec JaCoCo
                bat './mvnw jacoco:report'
            }
        }

        stage('Package') {
            steps {
                // Création du package JAR avec Maven
                bat './mvnw package'
            }
        }

        stage('Archive Artifacts') {
            steps {
                // Archivage des artefacts générés (fichiers JAR)
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        failure {
            // Envoi d'un email en cas d'échec du build
            mail to: 'elabjani.yassmine@gmail.com',
                 subject: ' Build Échoué - jhipster-sampla-app',
                 body: "Le build du projet jhipster-sampla-app a échoué. Veuillez consulter Jenkins pour plus de détails."
        }
    }
}


