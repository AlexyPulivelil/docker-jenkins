pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = 'techino'
    }

    stages {
        stage('Pull and Build Images Locally') {
            agent {
                docker {
                    image 'docker:latest'
                    args '--privileged -v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps {
                script {
                    // Pull latest images
                    docker.image('postgres:latest').pull()
                    docker.image('redis:latest').pull()
                    docker.image('mongo:latest').pull()
                    docker.image('mongo-express:latest').pull()

                    // Build images locally
                    // docker.build('my-postgres-image', './path/to/postgres-dockerfile')
                    // docker.build('my-redis-image', './path/to/redis-dockerfile')
                    // docker.build('my-mongo-image', './path/to/mongo-dockerfile')
                    // docker.build('my-mongo-express-image', './path/to/mongo-express-dockerfile')
                }
            }
        }

        stage('Push Images to Docker Hub') {
            agent {
                docker {
                    image 'docker:latest'
                    args '--privileged -v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials') {
                        docker.image("my-postgres-image").push("${DOCKER_HUB_REPO}/my-postgres-image:latest")
                        docker.image("my-redis-image").push("${DOCKER_HUB_REPO}/my-redis-image:latest")
                        docker.image("my-mongo-image").push("${DOCKER_HUB_REPO}/my-mongo-image:latest")
                        docker.image("my-mongo-express-image").push("${DOCKER_HUB_REPO}/my-mongo-express-image:latest")
                    }
                }
            }
        }
    }
}
