pipeline {
    agent any // Выбираем Jenkins агента, на котором будет происходить сборка: нам нужен любой

    triggers {
        pollSCM('H/5 * * * *') // Запускать будем автоматически по крону примерно раз в 5 минут
    }

    tools {
        maven 'maven-3.8.1' // Для сборки бэкенда нужен Maven
        jdk 'jdk16' // И Java Developer Kit нужной версии
        nodejs 'node16' // А NodeJS нужен для фронтафффdasa
    }
    environment {
        SLACK_CHANEL = 'jenkins-ci-noticed'
        SLACK_CREDENTIAL_ID = 'OAuth Access Token' // ID вашего Slack token credentials
        SLACK_BASE_URL = 'https://app.slack.com/client/T074CMMLGE7/D0751JU9MH6'
    }
    stages {
        stage('Build & Test backend') {
            steps {
                dir("backend") { // Переходим в папку backend
                    sh 'mvn package' // Собираем мавеном бэкенд
                }
            }

           post {
                success {
                    slackSend(
                        baseUrl: "${env.SLACK_BASE_URL}", 
                        channel: "${env.SLACK_CHANNEL}", 
                        color: 'good', 
                        message: "Backend build succeeded"
                    )
                    junit 'backend/target/surefire-reports/**/*.xml'
                }
                failure {
                    slackSend(
                        baseUrl: "${env.SLACK_BASE_URL}", 
                        channel: "${env.SLACK_CHANNEL}", 
                        color: 'danger', 
                        message: "Backend build failed"
                    )
                }
            }
        }    

        stage('Build frontend') {
            steps {
                dir('frontend') {
                    sh 'npm install --legacy-peer-deps'
                    sh 'npm run build'
                }
            }

            post {
                success {
                    slackSend(
                        baseUrl: "${env.SLACK_BASE_URL}", 
                        channel: "${env.SLACK_CHANNEL}", 
                        color: 'good', 
                        message: "Frontend build succeeded"
                    )
                }
                failure {
                    slackSend(
                        baseUrl: "${env.SLACK_BASE_URL}", 
                        channel: "${env.SLACK_CHANNEL}", 
                        color: 'danger', 
                        message: "Frontend build failed"
                    )
                }
            }
        }
        
        stage('Save artifacts') {
            steps {
                archiveArtifacts(artifacts: 'backend/target/sausage-store-0.0.1-SNAPSHOT.jar')
                archiveArtifacts(artifacts: 'frontend/dist/frontend/*')
            }
        }
    }
} 