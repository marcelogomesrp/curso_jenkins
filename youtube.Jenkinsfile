pipeline {
    agent any
    environment {
        BRANCH = 'Master'    
    }
    stages {
        stage('Step1') {
            environment {
                OUTRO = 'Outro valor'
            }
            steps {
                echo 'step 1'
                sh 'printenv'
            }
        }
        stage('Step2') {
            steps {
                echo 'step 2'
                sh 'printenv'
            }
        }
    }
}