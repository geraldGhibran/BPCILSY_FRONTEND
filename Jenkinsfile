env.DOCKER_REGISTRY = 'vanillavladimir'
env.DOCKER_IMAGE_NAME = 'client-prod'
node('master') {
    stage('Clean Workspace') {
      cleanWs()
    }
    stage('Git Pull from Github') {
        sh "git clone https://github.com/geraldGhibran/BPCILSY_FRONTEND.git"
    }
    stage('Build Docker Image') {
        sh "cd BPCILSY_FRONTEND/ && docker build --build-arg APP_NAME=client -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER} ."   
    }
    stage('Push Docker Image to Dockerhub') {
        sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
    }
    stage('DeployTo Kubernetes Cluster') {
        sh'''cd BPCILSY_FRONTEND/ && sed -i "15d" front-end-deployment-prod.yml'''
        sh'''cd BPCILSY_FRONTEND/ && sed -i "14 a \'\\'          image: vanillavladimir/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}" front-end-deployment-prod.yml && sed -i "s/''//" front-end-deployment-prod.yml'''
        sh "cd BPCILSY_FRONTEND/ && kubectl replace -f front-end-deployment-prod.yml -n client-prod --force"
   }
    stage('Remove Docker Image') {
        sh "docker rmi $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"   
    }
    stage('Clean Workspace') {
      cleanWs()
    }
}
