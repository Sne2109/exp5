pipeline{
    agents any

    environment{
        IMAGE_NAME="myapp"
        DOCKER_HUB_USER="sne2124"
    }
    stages{
        stage('checkout'){
            steps{
                echo "code already checked"
            }
        }
        stage('build image'){
            steps{
                sh '''
                docker build -t $DOCKER_HUB_USER/@IMAGE_NAME:latest .
                '''
            }
        }
        stage('login docker'){
            steps{
                withCredentials([usernamepassword(
                    credentialsid='dockerhub-creds'
                    usernamevariable='DOCKER_HUB_USER'
                    passwordvariable='DOCKER_PASS'
                    )]){
                        sh '''
                        echo $DOCKER_PASS|docker login -u $DOCKER_HUB_USER --password-stdin
                        '''
                        }
            }
        }
        stage(push image){
            steps{
                sh '''
                docker push $DOCKER_HUB_USER/@IMAGE_NAME:latest
                '''
            }
        }
        stage('Deploy container'){
            steps{
                sh '''
                docker stop myapp||true
                docker rm myapp||true
                docker run -d --name myapp 5000:5000 $DOCKER_HUB_USER/@IMAGE_NAME:latest 
                '''
            

        }
    }
}
}