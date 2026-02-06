node{
    try{
         stage('Checkout') {
            checkout scm
        }
        stage('init app'){
            bat 'echo iniciando mi app-express'
        }
        stage('check workspace') {
            bat 'dir'
        }
        stage("build docker image and push to registry"){
            bat 'docker build  -t app-express:v2 .'
            bat 'docker tag app-express:v2 jack2106/app-express:v2'
                withCredentials(
                    [usernamePassword(
                        credentialsId: 'id-token-docker',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS')]) {
                        bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                        bat 'docker push jack2106/app-express:v2'
            }
           
        }
        stage("deploy app to kubernetes"){
            bat 'minikube kubectl -- apply -f app-deployment.yaml'
            bat 'minikube kubectl -- apply -f app-service.yaml'
        }

    }catch(Exception e){
        throw e
    }
}