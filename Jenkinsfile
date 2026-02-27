pipeline {
    agent {
        label 'AGENT-1'
    }
     options {
               
                timeout(time: 1, unit: 'HOURS')
                disableConcurrentBuilds() 
            }
    // environment {
    //     DEPLOY_TO = 'production'
    //     GREETINGS = 'Chandra'
    // }
    stages {
        stage('Test') {
            steps {
                sh 'echo this is TEST'
        }
    }
    }
     post { 
        always { 
            echo 'I will always say Hello again!'
        }
        success { 
            echo 'I will always say Hello when pipeline success'
        }
        failure { 
            echo 'I will always say Hello when pipeline fails'
        }
    }

}