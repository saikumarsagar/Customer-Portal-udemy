pipeline {
    agent any
    environment {
        NETLIFY_AUTH_TOKEN = credentials("netlify-personal-access-token")
        NETLIFY_SITE_ID = '2ec587da-e684-4922-99a1-94e0db5ea3a8'
    }
    stages {
        stage("Checkout") {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/saikumarsagar/Customer-Portal-udemy.git']]])
            }
        }
        stage("Build") {
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
                stage('SCA') {
                    steps {
                        echo 'SCA'
                    }
                }
            }
        }
        stage("Deploy prod") {
            steps {
                nodejs(nodeJSInstallationName: 'node-lts') {
                    echo "Deploying site: ${NETLIFY_SITE_ID}"
                    sh 'node_modules/.bin/netlify status'
                    // Uncomment the line below if you want to deploy the site to production
                    // sh 'node_modules/.bin/netlify deploy --dir=build --prod'
                }
            }
        }
    }
    post {
        always {

                  if (fileExists('reports/junit.xml')) {
                    junit 'reports/junit.xml'
                } else {
                    echo 'JUnit XML file not found.'
                }
        }
    } 
}
