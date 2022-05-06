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
            echo "Dockerfile push Successfull"
            
        }
        
    }
    //first install ssh agent (for run command over ssh)
    stage("Deploy on Docker Host Server"){
        def dockerrun = 'docker run -p 8000:80 -dit --name mywebcon mm804/pipeline1:latest'
        sshagent(['dockerhostpassword']) {
          sh "ssh -o StrictHostKeyChecking=no -l ec2-user 172.31.3.107 ${dockerrun}"
        }
    }
    
}
