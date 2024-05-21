pipeline {
    agent any // Выбираем Jenkins агента, на котором будет происходить сборка: нам нужен любой
    
    tools {
        maven 'maven-3.8.1' // Для сборки бэкенда нужен Maven
        jdk 'jdk16' // И Java Developer Kit нужной версии
        nodejs 'node-16' // А NodeJS нужен для фронтаффф  попробуем
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
                    junit 'backend/target/surefire-reports/**/*.xml' // Передадим результаты тестов в Jenkins
                }
            }
        }

        stage('Build frontend') {
            steps {
                dir("frontend") {
                    sh 'npm install' // Для фронта сначала загрузим все сторонние зависимости
                    sh 'npm run build' // Запустим сборку  ЫЫЫЫААААА
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