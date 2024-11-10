pipeline {
    agent any

    environment {
        NETLIFY_AUTH_TOKEN = credentials('nfp_JHXsWfYSdkNcosxG1TWqU5eBpcNyWYg713a8') // Jenkins credential ID for Netlify token
    }

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
                    test -f build/index.html
                    npm test
                '''
            }
            post {
                always {
                    junit 'src/tests-results/Junit.xml'
                }
            }
        }

        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install -g netlify-cli // Global install with unsafe-perm
                    node_modules/.bin/netlify --version  // Run netlify
                    DEPLOY_URL=$(netlify deploy --prod --dir=build --auth=$NETLIFY_AUTH_TOKEN | grep "Live URL:" | awk '{print $3}')
                    echo "App deployed at: $DEPLOY_URL"
                '''
            }
        }
    }
}
