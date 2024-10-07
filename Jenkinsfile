pipeline {
    agent any

    tools {
        gradle 'Gradle_8.10'
    }

    stages {
        stage('Check for Changes') {
            steps {
                script {
                    // 현재 커밋 해시 가져오기
                    def currentCommit = sh(script: 'git rev-parse HEAD', returnStdout: true).trim()

                    // 이전 빌드의 커밋 해시 가져오기 (없으면 빈 문자열)
                    def previousCommit = sh(script: 'git rev-parse HEAD~1 || echo ""', returnStdout: true).trim()

                    def changes = sh(script: "git diff --name-only ${previousCommit} ${currentCommit} -- service/admin-service", returnStdout: true).trim()
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