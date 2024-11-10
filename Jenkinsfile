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
            # Ensure we're not in a directory that expects package.json
            npm install -g --unsafe-perm netlify-cli --prefix /usr/local  # Install netlify-cli globally

            # Verify netlify-cli was installed
            /usr/local/bin/netlify --version

            # Deploy to Netlify
            DEPLOY_URL=$(netlify deploy --prod --dir=build --auth=$NETLIFY_AUTH_TOKEN | grep "Live URL:" | awk '{print $3}')
            echo "App deployed at: $DEPLOY_URL"
        '''
            }
        }
    }
}
