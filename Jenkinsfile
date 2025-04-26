pipeline {
    agent any

    environment {
        MAVEN_OPTS = "-Dmaven.test.failure.ignore=false"  // Permet d'ignorer les erreurs de test
        MAVEN_HOME = tool 'Maven 3.9.3'
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
                //bat 'cmd /c mvnw.cmd clean compile'  // Utilisation de mvnw.cmd au lieu de mvnw
                //bat './mvnw clean compile'
                //bat './mvnw.cmd clean compile'  // Utilisation du chemin relatif
              bat 'cmd /c mvnw.cmd clean compile'  // Utilisez cette commande au lieu de './mvnw.cmd'


             
            }
        }

        stage('Test') {
            steps {
                // Exécution des tests unitaires avec Maven
                bat 'cmd /c mvnw.cmd test'  // Assurez-vous d'utiliser la commande correcte pour Windows
            }
        }

        stage('JaCoCo Report') {
            steps {
                // Génération du rapport de couverture des tests avec JaCoCo
                bat 'cmd /c mvnw.cmd jacoco:report'
            }
        }

        stage('Package') {
            steps {
                // Création du package JAR avec Maven
                bat 'cmd /c mvnw.cmd package'
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



