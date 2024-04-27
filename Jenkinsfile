pipeline {
    agent {
        label 'AGENT1'
    }
    options {
        ansiColor('xterm')
    }
    environment {
        packageVersion= ''
        nexusUrl=':8081'
    }
   
    stages{
        stage('get the version') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    packageVersion = packageJson.version
                    echo "application version is : $packageVersion"
                }
            }
        }
        stage('Install dependencies'){
            steps{
                sh """
                    npm install
                """
            }
        }
        stage('Unit Testing '){
            steps{
                sh """
                    echo "unit testing will run here"
                """
            }
        }
        stage('sonar scan'){
            steps{
                sh """ 
                    sonar-scanner
                """
            }
        }
        stage('Build'){
            steps{
                sh """
                    ls -la
                    zip -q -r catalogue.zip ./* -x ".git" -x ".zip"
                    ls -ltr
                """
            }
        }
        stage('Deploy'){
            steps{
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: '$nexusUrl',
                    groupId: 'com.roboshop',
                    version: "$packageVersion",
                    repository: 'catalogue',
                    credentialsId: 'nexus-auth',
                    artifacts: [
                        [artifactId: catalogue,
                        classifier: '',
                        file: 'catalogue' + $packageVersion + '.zip',
                        type: 'zip']
                    ]
                )
            }
        }
    }
    post { 
        always { 
            echo 'I will always say Hello again!!'
            deleteDir()
        }
        failure { 
            echo 'this runs when pipeline is failed, used generally to send some alerts'
        }
        success{
            echo 'I will say Hello when pipeline is success'
        }
    }
}