node{
    
    stage("Pull Code From Git"){
        git branch: 'main', url: 'https://github.com/mukesh804/docker-demo.git'
    }
    
    stage("Build Docker File"){
        sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID mm804/$JOB_NAME:v1.$BUILD_ID'
        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID mm804/$JOB_NAME:latest'
        
    }
    
    stage("Push To Docker HUB"){
        withCredentials([string(credentialsId: 'dockerhubpassword', variable: 'dockerhubpassword')]) {
            sh 'docker login -u mm804 -p ${dockerhubpassword}'
            sh 'docker image push mm804/$JOB_NAME:v1.$BUILD_ID'
            sh 'docker image push mm804/$JOB_NAME:latest'
            sh 'docker image rm $JOB_NAME:v1.$BUILD_ID mm804/$JOB_NAME:latest mm804/$JOB_NAME:v1.$BUILD_ID'
            
        }
        
    }
        
    
    
}
