pipeline {
    agent any

    environment {
        MAVEN_HOME = tool 'Maven 3.9.3'  // Assure-toi que Maven 3.9.3 est configuré dans Jenkins
    }

    options {
        skipStagesAfterUnstable()  // Ignore les étapes suivantes si le build devient instable
        timestamps()  // Ajoute des horodatages à chaque étape du log
    }

    stages {
        stage('ScrutationSCM') {
            steps {
                script {
                    // Clonage du dépôt et récupération des références Git (utilisation de git en mode batch)
                    bat '''git fetch --tags --force --progress -- https://github.com/yasselab/jhipster-sampla-app/ +refs/heads/*:refs/remotes/origin/*'''
                }
            }
        }

        stage('Compilation avec Maven') {
            steps {
                script {
                    // Compilation avec Maven (commande batch)
                    bat '''mvnw clean compile'''
                }
            }
        }

        stage('JUnit') {
            steps {
                script {
                    // Exécution des tests unitaires avec Maven (commande batch)
                    bat '''mvnw test'''
                    junit '**/target/surefire-reports/*.xml'  // Collecte les résultats des tests JUnit
                }
            }
        }

        stage('Jacoco Report') {
            steps {
                script {
                    // Exécution du rapport de couverture avec Jacoco (commande batch)
                    bat '''mvnw jacoco:report'''
                }
            }
        }

        stage('Intégration des PR') {
            steps {
                script {
                    // Affiche les derniers commits sur la branche (commande batch)
                    bat '''git log --oneline -5'''
                }
            }
        }

        stage('Packaging') {
            steps {
                script {
                    // Création du fichier JAR (commande batch)
                    bat '''mvnw package'''
                }
            }
        }
    }

    post {
        success {
            script {
                // Exécution après un succès
                echo "Pipeline exécuté avec succès sur ${env.BRANCH_NAME}"
            }
        }
        failure {
            script {
                // Envoi d'un email en cas d'échec
                mail to: 'elabjani.yassmine@gmail.com',
                     subject: "Échec du build : ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                     body: """Le pipeline a échoué à l’étape : ${env.STAGE_NAME}
                               Branche : ${env.BRANCH_NAME}
                               Voir les logs ici : ${env.BUILD_URL}"""
            }
        }
    }
}

