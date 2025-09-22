pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/prakash006602/samsung-mobile-site.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t moses777/samsung-site:v1 .'
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh "docker push moses777/samsung-site:v1"
                }
            }
        }

        stage('Run Container (Local Test)') {
            steps {
                sh 'docker run -d -p 8081:80 --name samsung-site-test moses777/samsung-site:v1 || true'
            }
        }
    }
}
