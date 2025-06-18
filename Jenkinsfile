pipeline {
    agent any

    tools {
        maven 'Maven 3.9.5'
    }

    environment {
        IMAGE_NAME = 'devops-pets-backend'
        CONTAINER_NAME = 'backend'
        DB_URL = 'jdbc:postgresql://postgres:5432/petdb'
        DB_USER = 'petuser'
        DB_PASS = 'petpass'
        JPA_MODE = 'update'
        NETWORK = 'devops-pets_default'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Tsilispyr/devops-pets-backend.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Docker Run') {
            steps {
                sh '''
                  docker stop $CONTAINER_NAME || true
                  docker rm $CONTAINER_NAME || true
                  docker run -d --name $CONTAINER_NAME \
                    --network $NETWORK \
                    -p 8080:8080 \
                    -e SPRING_DATASOURCE_URL=$DB_URL \
                    -e SPRING_DATASOURCE_USERNAME=$DB_USER \
                    -e SPRING_DATASOURCE_PASSWORD=$DB_PASS \
                    -e SPRING_JPA_HIBERNATE_DDL_AUTO=$JPA_MODE \
                    $IMAGE_NAME
                '''
            }
        }
    }
}
