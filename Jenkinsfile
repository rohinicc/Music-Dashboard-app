pipeline{
    agent {
        label 'slave_node'
    }
    environment{
        DOCKER_IMAGE = 'musicapp'
        DOCKERHUB_USERNAME = 'rohinicc'
        DOCKERHUB_REPO = 'music-dashboard-app'
        VERSION = '$BUILD_ID'
        CONTAINER_NAME = 'app'
        CONTAINER_PORT = '8085'
        REQUEST_PORT = '80'
        DOCKERHUB_CRED ='dockerhub-cred'
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
            stage('Run docker container'){
                steps{
                    sh 'sudo docker run -it -d --name ${CONTAINER_NAME} -p ${CONTAINER_PORT}:${REQUEST_PORT} ${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO}:latest'
                }
                post{
                    always{
                        echo 'container is running'
                    }
                    success{
                        echo 'container is running successfully'
                    }
                    failure{
                        echo 'failed to run a container'
                    }
                }
            }
            stage('push docker image to dockerhub'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred',usernameVariable:'DOCKERHUB_USERNAME',passwordVariable:'DOCKERHUB_PASSWORD')]){
                    sh """
                    echo ${DOCKERHUB_PASSWORD} | sudo docker login -u ${DOCKERHUB_USERNAME} --password-stdin
                    sudo docker push ${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO}:${VERSION}
                    sudo docker push ${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO}:latest
                    """
                    
                }
            }
            }
            }

        }
    
