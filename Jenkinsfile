pipeline {
    agent any
    stages {
        stage("Checkout") {
            steps {
          
              git 'https://github.com/saikumarsagar/Customer-Portal-udemy.git'
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
                        echo 'Test'
                    }
                }
                stage('SCA') {
                    steps {
                        echo 'SCA'
                    }
                }
            }
        }

        stage("Deploy stage") {
            steps {
                echo "Deploy stage"
            }
        }
        stage("Deploy prod") {
            steps {
                echo "Deploy prod"
            }
        }
    }
}
