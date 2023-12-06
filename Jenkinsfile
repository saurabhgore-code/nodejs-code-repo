pipeline {
    agent any
    
    stages {
        
        stage("code"){
            steps{
                git url: "https://github.com/saurabhgore-code/nodejs-code-repo.git", branch: "main"
               
            }
        }
        stage("build and test"){
            steps{
                sh "docker build -t nodejs-image ."
                
            }
        }
       
        stage("push image to dockerhub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker tag nodejs-image ${env.dockerHubUser}/nodejs-image:v2"
                sh "docker push ${env.dockerHubUser}/nodejs-image:v2"
                }
            }
        }
        stage("deploy using kubernetes"){
            steps{
                sh "kubectl create -f nodejs-deployment.yml"
    }
      stage("create a nodeport service"){
      steps{
                sh "kubectl create -f nodejs-service.yml"
    }
}
}
}
