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

    }

    stages {
        stage('Test') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Test-2') {
            steps {
                sh '''
                    pwd
                    ls -lrt
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
