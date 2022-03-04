pipeline {
    agent any
    stages {
        stage('step1'){
            steps {
                echo 'step 1'
                sh 'sleep 3'
            }
        }
        stage('Step2 - Parallel'){
            parallel {
                stage('step2 A'){
                    steps {
                        echo 'step 2 A'
                        sh 'sleep 5'
                    }            
                }
                stage('step2 B'){
                    steps {
                        echo 'step 2 B'
                        sh 'sleep 3'
                    }            
                }
            }
        }
        stage('step3'){
            steps {
                echo 'stepe 3'
                sh 'sleep 2'
            }
        }        
    }
}