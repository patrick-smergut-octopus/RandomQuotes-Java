pipeline {
    agent any
    tools {
        maven 'Maven 3.8.1'
        jdk 'Java 9'
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }

        stage ('Build') {
            steps {
                sh 'mvn package'
            }
        }

        stage ('Deploy to Octopus') {
            steps {
                withCredentials([string(credentialsId: 'OctopusAPIKey', variable: 'APIKey')]) {
                    sh """
                        ${tool('Octo CLI')}/Octo push --package target/randomquotes.0.1.9.war --replace-existing --server http://192.168.101.156 --apiKey ${APIKey}
                        ${tool('Octo CLI')}/Octo create-release --project "Jenkins Tomcat Deploy Test" --server http://192.168.101.156 --apiKey ${APIKey}
                        ${tool('Octo CLI')}/Octo deploy-release --project "Jenkins Tomcat Deploy Test" --version latest --deployto "Jankins Pipeline" --server http://192.168.101.156 --apiKey ${APIKey}
                    """
                }
            }
        }
    }
}
