pipeline{
    agent any
    environment{
        DOCKER_IMAGE = 'musicapp'
        DOCKERHUB_USERNAME = 'rohinicc'
        DOCKERHUB_REPO = 'music-dashboard-app'
        VERSION = '$BUILD_ID'
        CONTAINER_NAME = 'app'
        CONTAINER_PORT = '8003'
        REQUEST_PORT = '80'
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
        stage('docker tag'){
            steps{
                sh """
                sudo docker tag ${DOCKER_IMAGE} ${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO}:${VERSION}
                sudo docker tag ${DOCKER_IMAGE} ${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO}:latest
                """
            }
        }
            stage('docker image'){
                steps{
                    sh 'sudo docker images'
                }
            }
            stage('Remove the old-contaier'){
                steps{
                    sh 'sudo docker rm -f ${CONTAINER_NAME}'
                }
                post{
                    success{
                        echo 'Old container removed successfully.'
                    }
                    failure{
                        echo 'container is not present.'
                        sh 'sudo docker run -it -d --name ${CONTAINER_NAME} -P ${CONTAINER_PORT}/${REQUEST_PORT} ${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO}:latest'

                    }
                }
                
            }
        }
    }
}