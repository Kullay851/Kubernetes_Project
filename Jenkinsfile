node{
    stage('Git Checkout'){
        git credentialsId: 'GITHUB_Kullay', url: 'https://github.com/Kullay851/Kubernetes_Project.git'
    }
    stage('sending docker file to docker server'){
        sshagent(credentials: ['docker']) {
         sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.43.197'
         sh 'scp /var/lib/jenkins/workspace/pipeline-demo/* ubuntu@172.31.43.197:/home/ubuntu'
        }
    }
    stage('Building docker image'){
         sshagent(credentials: ['docker']) {
         sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.43.197 cd /home/ubuntu'
         sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.43.197 docker image build -t $JOB_NAME:v1.$BUILD_ID . '
         }
    }
    stage('tagging name to an image'){
        sshagent(credentials: ['docker']) {
         sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.43.197 cd /home/ubuntu'
         sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.43.197 docker image tag $JOB_NAME:v1.$BUILD_ID kullay/$JOB_NAME:v1.$BUILD_ID '
         sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.43.197 docker image tag $JOB_NAME:v1.$BUILD_ID kullay/$JOB_NAME:latest '
        }
    }
    stage('pushing docker image to docker hub'){
          sshagent(credentials: ['docker']) {
            withCredentials([string(credentialsId: 'dockerhub_password', variable: 'dockerhub_password')]) {
                sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.43.197 docker login -u kullay -p ${dockerhub_password}"
                 sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.43.197 docker image push kullay/$JOB_NAME:v1.$BUILD_ID'
                 sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.43.197 docker image push kullay/$JOB_NAME:latest '
            }
        }
    }
}
