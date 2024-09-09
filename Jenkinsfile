pipeline{
    agent {
        label 'AGENT-1'

    }
    options{
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }

    // parameters{

    // }

    environment{
        JENKINS_URL='http://54.160.219.238:8080/'
        NEXUS_URL='http://44.204.30.237:8081/'
        APP_VERSION=''

    }

    stages {
        stage('Read Application Version') {
            steps {
                
                script {
                    // Read the JSON file
                    def jsonFile = readJSON file: 'package.json'
                    
                    // Extract the version from the JSON
                    def version = jsonFile.version

                    // Print the version (for debugging purposes)
                    echo "Application Version: ${version}"

                    // Set the version as an environment variable
                    env.APP_VERSION = version
                }
            }
        }
        stage('Install Dependencyies') {
            steps {
                sh '''
                    npm install
                    ls -lrt
                '''
            }
        }
        
        stage('Build') {
            steps {
                // Use the environment variable in your build file
                echo "Building application version ${env.APP_VERSION}"
                // Example build command that uses the version
                // sh "your-build-command --version=${env.APP_VERSION}"
                sh '''
                    zip backend-${APP_VERSION}.zip * -x Jenkinsfile
                '''
            }
        }
    }

    post{
        always { 
            echo 'I will always say Hello again!'
        }
        success {
            echo 'Sucess'
        }
        failure {
            echo 'Failure'
        }
    }
}
