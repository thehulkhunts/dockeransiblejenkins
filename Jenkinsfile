pipeline{
    agent any
    tools {
      maven 'maven'
    }
    environment {
      DOCKER_TAG = ${BUILD_NUMBER} /*getVersion()*/
    }
    stages{
        stage('SCM'){
            steps{
                git branch: 'master', 
                    url: 'https://github.com/javahometech/dockeransiblejenkins'
            }
        }
        
        stage('Maven Build'){
            steps{
                sh "mvn clean package"
            }
        }
        
       
        stage('Docker Build'){
            environment {
                DOCKERFILE = "kammana/hariapp:${DOCKER_TAG}"
                REGISTRY_CREDENTIALS = credentials('docker-cred')
            }
            steps{
             script {
                  sh 'docker build . -t ${DOCKERFILE}'
                  dockerImage = docker.image("${DOCKERFILE}")
                  docker.withRegistry('https://index.docker.io/v1/', "docker-cred") {
                   dockerImage.push()
                  }
             }
            }
        }
        
       /* stage('DockerHub Push'){
            steps{
                withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
                    sh "docker login -u kammana -p ${dockerHubPwd}"
                }
                
                sh "docker push kammana/hariapp:${DOCKER_TAG} "
            }
        } */
        
     /*   stage('Docker Deploy'){
            steps{
              ansiblePlaybook credentialsId: 'dev-server', disableHostKeyChecking: true, extras: "-e DOCKER_TAG=${DOCKER_TAG}", installation: 'ansible', inventory: 'dev.inv', playbook: 'deploy-docker.yml'
            }
        } */
    }
}

/*def getVersion(){
    def commitHash = sh label: '', returnStdout: true, script: 'git rev-parse --short HEAD'
    return commitHash
} */
