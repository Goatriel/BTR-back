pipeline {
    agent any
    tools {
        maven 'maven3' // Le nom défini dans Tools
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package' // Sur un serveur distant (souvent Linux), on utilise 'sh'
            }
        }
    }
}