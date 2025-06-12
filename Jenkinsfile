pipeline {
    agent any

    environment {
        NEWRELIC_API_KEY = credentials('newrelic-api-key')
        APP_ID = '12345678' // Replace with your actual New Relic application ID
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning Spring Boot Hello World app...'
                git url: 'https://github.com/ReenaSo16/spring-boot-hello-world.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the app (no deploy)...'
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Notify New Relic') {
            steps {
                echo 'Sending deployment log to New Relic...'
                sh """
                curl -X POST "https://api.newrelic.com/v2/applications/${APP_ID}/deployments.json" \\
                  -H "X-Api-Key:${NEWRELIC_API_KEY}" \\
                  -H "Content-Type: application/json" \\
                  -d '{
                        "deployment": {
                          "revision": "1.0.${BUILD_NUMBER}",
                          "changelog": "Code changes built on Jenkins",
                          "description": "Build #${BUILD_NUMBER}",
                          "user": "jenkins"
                        }
                      }'
                """
            }
        }
    }
}
