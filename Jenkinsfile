pipeline {
    agent {
        any {
            image 'maven:3.9.3-eclipse-temurin-17-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    options {
        skipStagesAfterUnstable()
    }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        //stage('Test') { 
        //    steps {
        //        sh 'mvn test' 
        //    }
        //}
        stage('Deliver') {
            steps {
                sh "chmod +x deliver.sh"
                sh './deliver.sh'
            }
        }
        stage('Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin docker.io'
                // sh 'sudo docker login -u lukas.slizik@gmail.com -p zkYhLz9aaCdvF3EX docker.io'
            }
        }
        stage('Push') {
            steps {
                sh 'sudo docker push lukasslizik/java-docker:latest'
            }
        }
    }
    post {
        always {
              sh 'sudo docker logout'
        }
    }
}
