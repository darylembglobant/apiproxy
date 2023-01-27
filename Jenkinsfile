pipeline {

    agent any
    environment {
        API_NAME = 'Mock-v1'
        API_ORG = 'jenkins-test-375820'
        FILE_ID = 'c11a5e2b-9ff5-4254-9208-6981da319148'
    }
    options {
        buildDiscarder logRotator( 
                    daysToKeepStr: '16', 
                    numToKeepStr: '10'
            )
    }

    stages {
        
        stage('Cleanup Workspace') {
            steps {
                deleteDir()
                sh """
                echo "Cleaned Up Workspace For Project"
                """
            }
        }

        stage('Code Checkout') {
            steps {
                checkout scm
                sh "ls -lat"
            }
        }

        stage(' Unit Testing') {
            steps {
                sh """
                echo "Running Unit Tests Edit"
                """
            }
        }

        stage('Code Analysis') {
            steps {
                sh """
                echo "Running Code Analysis"
                """
            }
        }

        stage('Build Deploy Code') {
            steps {
                    dir(API_NAME){
                        withMaven (maven:'maven'){
                                configFileProvider(
                                    [configFile(fileId: '${FILE_ID}', variable: 'SERVICE_ACCOUNT_FILE')]) {
                                    sh 'mvn install -Ptest -Dorg=${API_ORG} -Denv=${GIT_BRANCH} -Dfile=$SERVICE_ACCOUNT_FILE'
                            }
                        }
                    }
            }
        }
    }   
}
