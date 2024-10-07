pipeline {
    agent any

    tools {
        gradle 'Gradle 8.10'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/xxxzerone/demo-jenkins.git'
            }
        }
        stage('Build'){
            steps{
                echo 'Building..'
                sh 'gradle clean build'
            }
        }
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}