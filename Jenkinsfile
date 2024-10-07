pipeline {
    agent any

    environment {
        DOCKER_HUB_URL = "${params.docker_hub_url}"
        DOCKER_HUB_CREDENTIAL = "${params.docker_hub_credential}"
    }

    tools {
        gradle 'Gradle_8.10'
    }

    stages {
        stage('Check for changes') {
            steps {
                script {
                    // 현재 커밋 해시 가져오기
                    def currentCommit = sh(script: 'git rev-parse HEAD', returnStdout: true).trim()
                    echo "currentCommit: ${currentCommit}"

                    // 이전 빌드의 커밋 해시 가져오기 (없으면 빈 문자열)
                    def previousCommit = sh(script: 'git rev-parse HEAD~1 || echo ""', returnStdout: true).trim()
                    echo "previousCommit: ${previousCommit}"

                    def changes = sh(script: "git diff --name-only ${previousCommit} ${currentCommit}", returnStdout: true).trim()
                    echo "changes: ${changes}"
                    if (changes) {
                        echo '변동사항이 있습니다.'
                        echo "변경된 파일들: ${changes}"
                    } else {
                        echo '변동사항이 없습니다.'
                        currentBuild.result = 'ABORTED'
                        error '변동 사항이 없으므로 파이프라인을 중단합니다.'
                    }
                }
            }
        }

        stage('Gradle build') {
            steps {
                sh 'chmod +x ./gradlew'
                sh './gradlew bootJar --no-daemon'
                sh 'cp build/libs/*.jar ./'
            }
        }

        stage('Docker image build and push') {
            steps {
                echo "DOCKER_HUB_URL: ${DOCKER_HUB_URL}"
                echo "DOCKER_HUB_CREDENTIAL: ${DOCKER_HUB_CREDENTIAL}"
                echo "BUILD_NUMBER: ${BUILD_NUMBER}"

                script {
                    docker.withRegistry("http://${DOCKER_HUB_URL}", "${DOCKER_HUB_CREDENTIAL}") {
                        // Docker 이미지 빌드
                        def image = docker.build("demo-jenkins:${env.BUILD_NUMBER}")
                        image.push()
                    }
                }
            }
        }
    }
    post{
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
        always{
            echo "========always========"
        }
    }
}