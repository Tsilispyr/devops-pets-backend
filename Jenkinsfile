pipeline {
    agent any

    tools {
        maven 'Maven 3.9.5' // Βεβαιώσου ότι το έχεις εγκαταστήσει στο Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Tsilispyr/devops-pets-backend.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t devops-pets-backend .'
            }
        }

        stage('Docker Run') {
            steps {
                sh 'docker run -d -p 8080:8080 devops-pets-backend'
            }
        }
    }
}
