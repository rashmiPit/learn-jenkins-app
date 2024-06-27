pipeline{
    agent any

    stages{
        stage('Build'){
            agent {
                docker {
                    image 'node:20.13.1-alpine'
                    reuseNode true
                }
            }

            steps{
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm install
                    npm run build
                    ls -la
                '''
            }
        }

        stage('Test Stage'){
            sh 'echo "npm test" '
        } 
    }
}