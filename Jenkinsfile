pipeline 
{
    agent any // Jenkins will be able to select all available agents
    stages 
    {
        stage('Docker Login')
        { //we pass the built image to our docker hub account
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
        stage(' Docker Build')
        { // docker build image stage
            steps {
                script {
                sh '''
                echo ""
                echo "-----  Build nginx service"
                #docker image build webapp/nginx-service/ -t nginx-service:latest
                #docker tag nginx-service ilhemb/nginx-service:latest
                #docker image push ilhemb/nginx-service:latest

                echo "-----  Build movie service"
                #docker image build webapp/movie-service/ -t movie-service:latest
                #docker tag movie-service ilhemb/movie-service:latest
                #docker image push ilhemb/movie-service:latest

                echo "-----  Build cast service"
                #docker image build webapp/cast-service/ -t cast-service:latest
                #docker tag cast-service ilhemb/cast-service:latest
                #docker image push ilhemb/cast-service:latest                                
                '''
                }
            }
        }
        stage('Deploiement en dev')
        {
            environment
            {
                KUBECONFIG = credentials("config") // we retrieve  kubeconfig from secret file called config saved on jenkins
                NAMESPACE: dev
                CHARTNAME: microservice-fastapi-dev                
                NODEPORT_NGINX: 3000
                CLUSTERIP_MOVIE: 10.43.50.0
                CLUSTERIP_CAST: 10.43.50.1
            }
            steps {
                script {
                sh '''
                cat $KUBECONFIG > k8s_config
                helm upgrade --kubeconfig k8s_config --install $CHARTNAME  helm/microservice-fastapi/ --values=helm/microservice-fastapi/values.yaml --set nameSpace="$NAMESPACE"  --set nginx.service.nodePort="$NODEPORT_NGINX" --set movie.service.clusterIP="$CLUSTERIP_MOVIE" --set cast.service.clusterIP="$CLUSTERIP_CAST"
                '''
                }
            }
        }
    }
}