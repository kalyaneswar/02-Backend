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
        def appVersion = ''

    }

    stages {
        stage('Read Application Version') {
            steps {
                
                script {
                    // Read the JSON file
                    def jsonFile = readJSON file: 'package.json'
                    
                    // Extract the version from the JSON
                    appVersion = jsonFile.version

                    // Print the version (for debugging purposes)
                    echo "Application Version: ${appVersion}"

                    // Set the version as an environment variable
                    // APP_VERSION = version
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
                echo "Building application version ${appVersion}"
                // Example build command that uses the version
                // sh "your-build-command --version=${env.APP_VERSION}"
                sh '''
                    zip -q -r backend-${appVersion}.zip * -x Jenkinsfile backend-${appVersion}.zip
                    pwd
                    ls -lrt
                '''
            }
        }
    }

    post{
        always { 
            echo 'I will always say Hello again!'
            deleteDir()
        }
        success {
            echo 'Sucess'
        }
        failure {
            echo 'Failure'
        }
    }
}
