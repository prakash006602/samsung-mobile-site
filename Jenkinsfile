pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
               git branch: 'main', url:'https://github.com/siva-123-hash/samsung-mobile-site.git'
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
                    sh 'docker build -t siva0927/samsung-site:v1 .'
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'siva0927-dockerhub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh "docker push siva0927/samsung-site:v1"
                }
            }
        }

        stage('Run Container (Local Test)') {
            steps {
                sh 'docker run -d -p 8081:80 --name samsung-site-test siva0927/samsung-site:v1 || true'
            }
        }

        stage('Deploy to Docker Swarm') {
            steps {
                sh '''
                docker service rm samsung-site || true
                docker service create --name samsung-site -p 8081:80 siva0927/samsung-site:v1
                '''
            }
        }
    }
}


