env.DOCKER_REGISTRY = 'vanillavladimir'
env.DOCKER_IMAGE_NAME = 'client-staging'
node('master') {
    stage('Clean Workspace') {
      cleanWs()
    }
    stage('Git Pull from Github') {
        sh "git clone https://github.com/geraldGhibran/BPCILSY_FRONTEND.git"
    }
    stage('Build Docker Image') {
        sh "cd /var/lib/jenkins/BPCILSY/mern-todo-app/ && docker build --build-arg APP_NAME=client-staging -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER} ."   
    }
    stage('Push Docker Image to Dockerhub') {
        sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
    }
    stage('DeployTo Kubernetes Cluster') {
        sh'''cd /var/lib/jenkins/BPCILSY/mern-todo-app/ && sed -i "15d" front-end-deployment-staging.yml'''
        sh'''cd /var/lib/jenkins/BPCILSY/mern-todo-app/ && sed -i "14 a \'\\'          image: vanillavladimir/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}" front-end-deployment-staging.yml && sed -i "s/''//" front-end-deployment-staging.yml'''
        sh "cd /var/lib/jenkins/BPCILSY/mern-todo-app/ && kubectl replace -f front-end-deployment-staging.yml -n client-staging --force"
   }
    stage('Remove Docker Image') {
        sh "docker rmi $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"   
    }
    stage('Clean Workspace') {
      cleanWs()
    }
}
