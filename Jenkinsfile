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
        JENKINS_URL='http://jenkins.kalyaneswar.online:8080/'
        NEXUS_URL='nexus.kalyaneswar.online:8081'
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
        stage('Build'){
            steps {
                // Use the environment variable in your build file
                // echo "Building application version ${appVersion}"
                // Example build command that uses the version
                // sh "your-build-command --version=${env.APP_VERSION}"
                sh """
                zip -q -r backend-${appVersion}.zip * -x Jenkinsfile -x backend-${appVersion}.zip
                pwd
                ls -lrt
                """
            }
        }
        stage('Nexus ArtifactUploader') {
            steps{
                 nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: "${NEXUS_URL}",
                    groupId: 'com.expense',
                    version: "${appVersion}",
                    repository: 'backend',
                    credentialsId: 'nexus-auth',
                    artifacts: [
                        [artifactId: "backend",
                        classifier: '',
                        file: 'backend-' + "${appVersion}" + '.zip',
                        type: 'zip']
                         ]
                    )
            }
        }
        // Upstream Pipeline: After building and uploading the artifact, 
        // it triggers the downstream pipeline (CD-Pipeline-Job) and passes the appVersion parameter.
        stage('Trigger CD Pipeline') {
            steps {
                build job: '02.01-Backend-down_stream-', // Name of your downstream job
                parameters: [
                    string(name: 'appVersion', value: "${appVersion}")
                ]
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