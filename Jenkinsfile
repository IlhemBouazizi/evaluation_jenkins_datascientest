pipeline {
agent any // Jenkins will be able to select all available agents
stages {
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