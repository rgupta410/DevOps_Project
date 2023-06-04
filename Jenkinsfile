node{
    stage('Git Checkout'){
        git 'https://github.com/rgupta410/DevOps_Project.git'
    }
    stage("Docker Build"){
            sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
            sh 'docker image tag $JOB_NAME:v1.$BUILD_ID rgupta410/$JOB_NAME:v1.$BUILD_ID'
            sh 'docker image tag $JOB_NAME:v1.$BUILD_ID rgupta410/$JOB_NAME:latest'

         }
         stage("push Image: DOCKERHUB"){
            withCredentials([string(credentialsId: 'dockerpass', variable: 'DockerPsswd')]) {
                sh "docker login -u rgupta410 -p ${DockerPsswd}"
                sh 'docker image push rgupta410/$JOB_NAME:v1.$BUILD_ID'
                sh 'docker image push rgupta410/$JOB_NAME:latest'
                sh 'docker image rm $JOB_NAME:v1.$BUILD_ID rgupta410/$JOB_NAME:v1.$BUILD_ID rgupta410/$JOB_NAME:latest'
              }
         }
         stage('Docker container Deployment'){
             def docker_run = 'docker run -d --name scriptedcontainer -p 9000:80 rgupta410/webapp_pipeline'
             def docker_rmv_cont = 'docker rm -f scriptedcontainer'
             def docker_rmi = 'docker rmi -f rgupta410/webapp_pipeline'
                sshagent(['webapp']) {
                    sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.6.102 ${docker_rmv_cont}"
                    sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.6.102 ${docker_rmi}"
                    sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.6.102 ${docker_run}"

}
         }
}
