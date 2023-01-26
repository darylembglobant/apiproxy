pipeline {

    agent any

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
            when {
                branch 'master'
            }
            steps {
                    withMaven (maven:'maven'){
                            configFileProvider(
                                [configFile(fileId: 'e463a33a-7a9e-4a90-ae5d-2129037386c8', variable: 'SERVICE_ACCOuNT_FILE')]) {
                                sh 'mvn install -Ptest -Dorg=dynamic-branch-375904 -Denv=eval -Dfile=$SERVICE_ACCOuNT_FILE'
                        }
                    } // withMaven will discover the generated Maven artifacts, JUnit Surefire & FailSafe reports and FindBugs reports
                }
            
        }

    }   
}