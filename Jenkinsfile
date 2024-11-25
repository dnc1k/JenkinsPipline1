pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/your-username/your-repo.git' // URL вашего репозитория
        DOCKER_IMAGE = 'your-dockerhub-username/your-app' // Имя Docker-образа
        DOCKER_CREDENTIALS = 'docker-hub-credentials-id' // ID учётных данных Docker Hub в Jenkins
    }

    stages {
        stage('Клонирование репозитория') {
            steps {
                echo 'Клонируем репозиторий...'
                git branch: 'main', url: "${REPO_URL}" // Клонируем репозиторий из указанной ветки
            }
        }

        stage('Установка зависимостей') {
            steps {
                echo 'Устанавливаем зависимости...'
                // Пример для Node.js проекта
                sh 'npm install' // Запускаем установку зависимостей
            }
        }

        stage('Выполнение тестов') {
            steps {
                echo 'Запускаем тесты...'
                sh 'npm test' // Запуск тестов
            }
        }

        stage('Сборка Docker-образа') {
            steps {
                echo 'Собираем Docker-образ...'
                sh "docker build -t ${DOCKER_IMAGE}:latest ." // Сборка Docker-образа
            }
        }

        stage('Публикация Docker-образа') {
            steps {
                echo 'Публикуем Docker-образ...'
                withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin' // Авторизация в Docker
                }
                sh "docker push ${DOCKER_IMAGE}:latest" // Публикация Docker-образа
            }
        }
    }

    post {
        success {
            echo 'Пайплайн выполнен успешно!'
        }
        failure {
            echo 'Пайплайн завершился с ошибкой.'
        }
        always {
            echo 'Завершение пайплайна. Очистка ресурсов.'
        }
    }
}
