pipeline {
agent any // Jenkins will be able to select all available agents
stages {
        stage('Docker Login'){ //we pass the built image to our docker hub account
            environment
            {
                DOCKER_PASS = credentials("DOCKER_PASS") // we retrieve  docker password from secret text called docker_hub_pass saved on jenkins
                DOCKER_ID = 'ilhemb'
            }
            steps {
                script {
                sh '''
                docker login -u $DOCKER_ID -p $DOCKER_PASS
                '''
                }
            }
        }
        stage(' Docker Build'){ // docker build image stage
            steps {
                script {
                sh '''
                echo ""
                echo "-----  Build nginx service"
                docker image build webapp/nginx-service/ -t nginx-service:latest
                docker tag nginx-service ilhemb/nginx-service:latest
                docker image push ilhemb/nginx-service:latest
                '''
                }
            }
        }



}
}