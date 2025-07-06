pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/Harshvardhangupta-32/aws-jenkins'
        DOCKER_IMAGE = 'yourdockerhub/flask-app'
        DEPLOY_SERVER = 'ec2-user@54.252.135.229'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git REPO_URL
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ${DOCKER_IMAGE}:latest .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login -u "$hrshgupta" --hrshgit2005-stdin
                        docker push ${DOCKER_IMAGE}:latest
                    '''
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                script {
                    sshagent(['deploy-key']) {
                        sh '''
                            ssh ${DEPLOY_SERVER} "docker pull ${DOCKER_IMAGE}:latest && docker run -d -p 5000:5000 ${DOCKER_IMAGE}:latest"
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
    }
}
