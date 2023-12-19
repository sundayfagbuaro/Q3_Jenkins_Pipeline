node{
    stage('SCM Checkout'){
        git credentialsId: 'git', url: 'https://github.com/sundayfagbuaro/Project_DevOps_With_Kay.git'
    }
    stage('Build Docker Image'){
        sh 'docker build -t sundayfagbuaro/testapp:v1.0 .'
    }
    stage('Push Image to Docker Hub'){
        withCredentials([string(credentialsId: 'docker-pwd', variable: 'DockerHubPwd')]) {
            sh 'docker login -u sundayfagbuaro -p ${DockerHubPwd}' 
        }
        sh 'docker push sundayfagbuaro/testapp:v1.0'
    }
    stage('Deploy Container on The Devserver'){
        sshagent(['devserver_bobosunne']) {
            sh 'ssh -o StrictHostKeyChecking=no bobosunne@192.168.1.85'
            sh 'docker run -d -p 8081:80 --name testapp_new sundayfagbuaro/testapp:v1.0'
        }
    }
}