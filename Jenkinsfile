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
    }
}
