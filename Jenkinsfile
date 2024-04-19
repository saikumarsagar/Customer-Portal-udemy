pipeline {
    agent any

    environment {
        NETLIFY_AUTH_TOKEN = credentials("netlify-personal-access-token")
        NETLIFY_SITE_ID = '207d32f8-8436-4c57-a3ec-77bae0d02b4a'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/saikumarsagar/Customer-Portal-udemy.git'
            }
        }
        stage('Build') {
            steps {
                nodejs(nodeJSInstallationName: 'node-lts') {
                    sh 'node --version'
                    sh 'npm --version'
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Parallel Stage') {
            failFast true
            parallel {
                stage('Test') {
                    steps {
                        nodejs(nodeJSInstallationName: 'node-lts') {
                            sh 'npm test -- --reporters=jest-junit'
                        }
                    }
                }
                // Uncomment and configure SCA stage if needed
                /*
                stage('SCA') {
                    steps {
                        snykSecurity(
                            snykInstallation: 'snyk@latest',
                            snykTokenId: 'snyk-api-token',
                        )
                    }
                }
                */
            }
        }

        // Uncomment and configure Deploy staging stage if needed
        /*
        stage('Deploy staging') {
            steps {
                nodejs(nodeJSInstallationName: 'node-lts') {
                    sh 'node_modules/.bin/netlify deploy --dir=build'
                }
            }
        }
        */

        stage('Deploy prod') {
            steps {
                nodejs(nodeJSInstallationName: 'node-lts') {
                    echo "Deploying site: ${NETLIFY_SITE_ID}"
                    sh 'node_modules/.bin/netlify status'
                    // Uncomment the following line to deploy to production
                    // sh 'node_modules/.bin/netlify deploy --dir=build --prod'
                    // Uncomment and configure the following line to perform post-deployment validation
                    // sh 'curl https://YOUR-SITE-NAME.netlify.app/ | grep -q "React App"'
                }
            }
        }
    }

    post {
        always {
            junit 'reports/junit.xml'
        }
        success {
            archiveArtifacts artifacts: 'build/**', fingerprint: true
        }
    }
}
