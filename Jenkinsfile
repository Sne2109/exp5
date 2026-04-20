pipeline {
    agent any
    environment {
        IMAGE_NAME= "sneha"
        DOCKER_HUB_USER = "sne2124"
        FULL_IMAGE = "${DOCKER_HUB_USER}/${IMAGE_NAME}:latest"   
    }

    stages {
        stage("Checkout") {
            steps {
                echo "Checkout is done"
            }
        }

        stage('Build docker image') {
            steps{
                sh '''
                docker build -t $FULL_IMAGE .
                '''
            }
        }

        stage('Login to docker hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]){
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    '''
                }
            }
        }
        stage('Push image') {
            steps {
                sh '''
                docker push $FULL_IMAGE
                '''
            }
        }
        stage('deploy container') {
            steps {
                sh '''
                docker stop myapp || true
                docker rm myapp || true
                docker run -d --name myapp -p 5000:5000 $FULL_IMAGE
                '''
            }
        }
    }
    post {
        success {
            echo "pipeline completed"
        }
        failure {
            echo "pipeline failed"
        }
        always {
            sh ' docker logout || true '
        }
    }
}
