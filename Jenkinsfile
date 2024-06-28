pipeline {
    agent any

    parameters {
        string(name: 'DOCKER_IMAGE_NODE', defaultValue: 'node:20.13.1-alpine', description: 'Enter the node version')
    }

    environment {
        DOCKER_IMAGE_NODE = "${params.DOCKER_IMAGE_NODE}"
    }

    stages {
        stage('Prepare') {
            steps {
                script {
                    docker.image("${params.DOCKER_IMAGE_NODE}").pull()
                }
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    docker.image("${params.DOCKER_IMAGE_NODE}").inside('-u 0') {
                        commonSteps()
                        sh 'npm run build'
                        sh 'ls -la'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    docker.image("${params.DOCKER_IMAGE_NODE}").inside('-u 0') {
                        sh '''
                            test -f build/index.html
                            npm test
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}

def commonSteps() {
    sh '''
        ls -la
        node --version
        npm --version

        npm ci
        npm install
    '''
}
