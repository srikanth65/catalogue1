pipeline {
    agent {
        label 'AGENT1'
    }
    options {
        ansiColor('xterm')
    }
    environment {
        packageVersion= ''
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
            sh """
             npm install
            """
        }
    }
    post { 
        always { 
            echo 'I will always say Hello again!!'
        }
        failure { 
            echo 'this runs when pipeline is failed, used generally to send some alerts'
        }
        success{
            echo 'I will say Hello when pipeline is success'
        }
    }
}