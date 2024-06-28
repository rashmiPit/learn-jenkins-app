pipeline{
    agent any


    parameters{
        string(name: 'DOCKER_IMAGE_NODE', defaultValue: 'node:20.13.1-alpine', description: 'Enter the node version')
    }

    
    environment{
        DOCKER_IMAGE_NODE : "${params.DOCKER_IMAGE_NODE}"
    }

    options { 
        // Same node is used for the entire pipeline
        reuseNode true
    }


    stages{

        stage('Prepare'){
            steps{
                script{
                    docker.image(params.DOCKER_IMAGE_NODE).pull()
                }
            }
        }

        stage('Build'){
            steps{
                script{
                    docker.image(params.DOCKER_IMAGE_NODE).inside('-u 0'){
                        commonSteps()
                        sh 'npm run build'
                        sh 'ls -la'
                    }
                }
            }
        }

        stage('Test Stage'){
            steps{
                script{
                    docker.image(params.DOCKER_IMAGE_NODE).inside('-u 0')
                     // This is used to check the index.html file is avaible it the build directory or not
                    sh '''
                        test -f build/index.html
                        npm test
                    ''' 
                }
                
            }
        } 
    }

    post{
        always {
            junit 'test-results/junit.xml'
        }
    }
}

def commonSteps(){
    sh '''
        ls -la
        node --version
        npm --version

        npm ci
        npm install
    '''
}