pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = 'techino'
        KANIKO_IMAGE = 'gcr.io/kaniko-project/executor:latest'
    }

    stages {
        stage('Pull and Build Images Locally') {
            steps {
                script {
                    sh 'pwd'
                    // Define images to pull and build locally
                    def images = [
                        'postgres:latest',
                        'redis:latest',
                        'mongo:latest',
                        'mongo-express:latest'
                    ]

                    // Iterate through each image
                    images.each { imageName ->
                        // Pull the image using Kaniko
                        sh """
                            docker run --rm \
                                -v /var/run/docker.sock:/kaniko/docker.sock \
                                -v /root/.docker:/kaniko/.docker \
                                ${KANIKO_IMAGE} \
                                 --dockerfile=Dockerfile \
                                --destination=${DOCKER_HUB_REPO}/${imageName}
                        """
                    }
                }
            }
        }

        stage('Push Images to Docker Hub') {
            steps {
                script {
                    // Iterate again through each image for pushing
                    def images = [
                        'postgres:latest',
                        'redis:latest',
                        'mongo:latest',
                        'mongo-express:latest'
                    ]

                    // Push each image to Docker Hub
                    images.each { imageName ->
                        sh "docker push ${DOCKER_HUB_REPO}/${imageName}"
                    }
                }
            }
        }
    }
}
