pipeline {
    agent {
        label 'AGENT-1'
    }
     options {
               
                timeout(time: 1, unit: 'HOURS')
                disableConcurrentBuilds() 
                //ansiColor('xterm')
            }
    environment {
      def AppVersion = ''
      nexusURL = 'nexus.chandradevops.online:8081'
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
                zip -q -r backend-${AppVersion}.zip * -x Jenkinsfile -x backend-${AppVersion}.zip
                ls -ltr
                """
        }
    }
        stage('Nexus Artifact Upload'){
            steps {
                script {
                    nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: "${nexusURL}",
                    groupId: 'com.expense',
                    version: "${AppVersion}",
                    repository: 'backend',
                    credentialsId: 'nex-auth',
                    artifacts: [
                        [artifactId: "backend",
                        classifier: '',
                        file: 'backend-' + "${AppVersion}" + '.zip',
                        type: 'zip']
                    ]
                )
                }
            }
        }

        stage('Deploy'){
            steps {
                script {
                    def params = [
                    string(name: 'AppVersion', value:"${AppVersion}" )
                ]
                        build job: 'backend-deploy', parameters: params , wait: false
                }
            }
        }

    }
     post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir()
        }
        success { 
            echo 'I will always say Hello when pipeline success'
        }
        failure { 
            echo 'I will always say Hello when pipeline fails'
        }
    }

}