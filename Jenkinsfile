pipeline {
    // These are pre-build sections
    agent {
        node {
            label 'AGENT-1'
        }
    }
    environment{
        COURSE = "Jenkins"
        appVersion = ""
        ACC_ID = "160885265516"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"
    }
    options{
        timeout(time: 10, unit: 'MINUTES')
        disableConcurrentBuilds()
    }
    // This is build section
    stages {
        stage('ReadVersion') {
            steps {
                script{
                        def packageJSON = readJSON file: 'package.json'
                        env.appVersion = packageJSON.version
                        echo "app version: ${env.appVersion}"
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                script{
                    sh """
                        npm install
                    """
                }
            }
        }
        stage('Build Image') {
            steps {
                script{
                    withAWS(region:'us-east-1',credentials:'aws-creds'){
                        sh """
                            docker build -t catalogue:${env.appVersion} .
                        """
                    }
                }
            }
        }
    }
    post{
        always{
            echo 'I will always say Hello again'
            cleanWs()
        }
        success {
            echo 'I will run if success'
        }
        failure{
            echo 'I will run if failure'
        }
    }
}