pipeline{
    agent any
    environment{
        DOCKER_IMAGE='musicApp'
    }
    stages{
        stage('docker version'){
            steps{
                sh 'docker --version'
            }
        }
        stage('bulid docker image'){
            steps{
                sh 'sudo docker build -t ${DOCKER_IMAGE} .'
            }
        }
    }
}