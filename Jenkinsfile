pipeline {
    agent any

    stages {


        stage('Clean') {
            steps {
                sh 'mvn clean'
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }

        stage ('Building image'){
                steps{
                sh 'docker build -t azizk99/stock_backend:1.0 .'
                }
            }

        stage ('Push Docker Image'){

            steps{
             withCredentials([usernamePassword(credentialsId: 'dockerhub_cred', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
             sh "docker login -u \$DOCKER_USERNAME -p \$DOCKER_PASSWORD"
             sh 'docker push azizk99/stock_backend:1.0'
             }
            }
        }

        stage('Sonarqube') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'sonarqube-credentials', usernameVariable: 'SONAR_USERNAME', passwordVariable: 'SONAR_PASSWORD')]) {
                sh "mvn sonar:sonar -Dsonar.login=\$SONAR_USERNAME -Dsonar.password=\$SONAR_PASSWORD"
                }
            }
        }

        stage("Analysis Testing") {
            steps {
                sh 'mvn test'
            }
        }

        stage("Docker Compose") {
            steps {
                sh 'docker compose up -d'
            }
        }

        stage("Deploy to Nexus") {
            steps {
                sh 'mvn deploy'
            }
        }
    }
}
