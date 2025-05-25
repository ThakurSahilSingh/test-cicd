pipeline{
    agent any
    stages{
        stage('Clone'){
            steps{
                sh'''
                echo "Pipeline Started"
                '''
                git branch:'main', url:'https://github.com/ThakurSahilSingh/test-cicd.git'
            }
        }
        stage('build'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'docker-cred', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]){
                    sh'''
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    docker build -t sahil0824/myapp:latest .
                    docker push sahil0824/myapp:latest
                    docker logout
                    '''
                }
            }
        }
        stage('Deploy'){
            steps{
                withCredentials([file(credentialsId: 'k8s-cred', variable: 'KUBECONFIG_FILE')]) {
                    sh '''
                        export KUBECONFIG=$KUBECONFIG_FILE
                        kubectl get pods
                        kubectl apply -f deployment.yaml
                        kubectl apply -f service.yaml
                        kubectl get services
                    '''
            }
        }
    }
}
}
    
