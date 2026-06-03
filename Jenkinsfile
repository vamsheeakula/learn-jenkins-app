pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
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
                sh '''
                    ls -la
                    cd build
                    ls -la
                    cd ..
                    echo "Check if index.html is present"
                    test -f build/index.html
                    echo "RUNNING TESTS"
                    CI=true npm test
                '''
             }

        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}