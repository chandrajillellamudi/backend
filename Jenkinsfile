pipeline {
    agent {
        label 'AGENT-1'
    }
     options {
               
                timeout(time: 1, unit: 'HOURS')
                disableConcurrentBuilds() 
            }
    environment {
      def AppVersion = ''
    }
    stages {
        stage ('read the version') {
            steps {
                script {
                def packageJson = readJSON file: 'package.json'
                AppVersion = packageJson.version
                echo "Application Version: $AppVersion"
            }
            }
        }
        stage('Install dependencies') {
            steps {
                sh """
                npm install
                ls -ltr
                echo "AppVersion:${AppVersion}"
                """
        }
    }
        stage('Build') {
            steps {
                sh """
                zip -r backend-${AppVersion}.zip * -x Jenkinsfile -x backend-${AppVersion}.zip
                ls -ltr
                """
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