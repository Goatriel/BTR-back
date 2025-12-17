// Jenkinsfile-Backend
pipeline {
    // Utiliser un agent Docker pour garantir un environnement de build propre
    agent {
        docker {
            image 'maven:3.8.6-openjdk-17-slim' // Image Maven pour construire le projet Spring Boot
            label 'docker-agent' // Assurez-vous d'avoir un agent Docker configuré
        }
    }

    stages {
        stage('Checkout Code') {
            steps {
                // IMPORTANT : Cloner le dépôt du Back-end
                git branch: 'main', url: 'https://votre.git.url/budget-tracker-backend.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Démarrage du build Maven...'
                // Exécutez le build Maven : clean (nettoyage) et package (compilation et création du JAR)
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Unit Tests') {
            steps {
                echo 'Lancement des tests unitaires...'
                // Si vous aviez des tests, cette commande les exécuterait
                sh 'mvn test' 
                // Pour l'instant, on se contente d'un message pour valider l'étape.
            }
        }
        
        stage('Publish Artifact') {
             steps {
                echo 'L\'artefact est prêt : budget-tracker-backend.jar'
                // Optionnel : Archivage du JAR généré
                // archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Démarrage de l\'analyse SonarQube...'
                // Exécute l'analyse Maven en utilisant les propriétés Sonar
                // L'identifiant (ID) du jeton Jenkins et l'URL du serveur sont passés via les variables d'environnement Jenkins.
                withCredentials([string(credentialsId: 'SONARQUBE_TOKEN_BACKEND', variable: 'SONAR_TOKEN')]) {
                    sh "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.9.1.2184:sonar \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.token=${SONAR_TOKEN} \
                        -Dsonar.projectKey=budget-tracker-backend \
                        -Dsonar.projectName='Budget Tracker Backend'"
                }
            }
        }
    }
}
