pipeline {

    agent any 

    stages {
        stage('Build') {
            agent {
                docker {
                    image  'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh ''' 
                echo "helloworld"
                ls -la
                node --version
                npm --version
                npm ci
                npm run build
                ls -la
                '''
            }
        }

        stage('Test') {
           agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                
                echo "Test Stage"
                //  sh 'test -f build/index.html'
                sh 'npm test'
                
            }
        }

        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-noble'
                    reuseNode true
                }
            }
            steps {
                sh ''' 
                npm install serve
                node_modules/.bin/serve -s build &
                sleep 10
                npx playwright test
                '''
                
            }
        }

      
    }

      post {
        always {
            junit 'jest-results/junit.xml'
        }
    }
}