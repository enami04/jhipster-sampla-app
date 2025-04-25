pipeline {
    agent any

    environment {
        MAVEN_OPTS = "-Dmaven.test.failure.ignore=false"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/yasselab/jhipster-sampla-app.git'
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
                bat './mvnw jacoco:report'
            }
        }

        stage('Package') {
            steps {
                bat './mvnw package'
            }
        }

       
        
    }

    post {
        failure {
            mail to: 'elabjani.yassmine@gmail.com',
                 subject: ' Build Échoué ',
                 body: "Le build du projet a échoué. Veuillez consulter Jenkins pour plus de détails."
        }
    }
}
