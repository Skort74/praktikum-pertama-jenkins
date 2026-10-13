pipeline {
    agent any
    environment {
        // Ganti 'abdieeuh' dengan username Docker Hub Anda sendiri
        IMAGE_NAME = 'Skort74/simple-app'
        REGISTRY = 'https://index.docker.io/v1/'
        REGISTRY_CREDENTIALS = 'dockerhub-credentials'
        
        // Path standar untuk macOS (Apple Silicon via Homebrew /usr/local/bin atau /opt/homebrew/bin)
        PATH = "/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
    }
    stages {
        stage('Checkout') {
            steps {
                echo 'Melakukan checkout dari SCM...'
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Membangun Docker image di macOS...'
                    // Menggunakan Docker pipeline plugin untuk build image
                    def customImage = docker.build("${IMAGE_NAME}:${env.BUILD_NUMBER}")
                    echo "Image berhasil dibuat: ${IMAGE_NAME}:${env.BUILD_NUMBER}"
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    echo 'Push image ke Docker Hub...'
                    docker.withRegistry(REGISTRY, REGISTRY_CREDENTIALS) {
                        def customImage = docker.image("${IMAGE_NAME}:${env.BUILD_NUMBER}")
                        customImage.push()
                        customImage.push('latest')
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'Selesai build pipeline di MacBook M1!'
        }
        success {
            echo 'Build Berhasil 🎉'
        }
        failure {
            echo 'Build Gagal, silakan cek console log ❌'
        }
    }
}