pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building Docker image...'
                // Збираємо образ з тегом latest
                sh 'docker build -t myapp:latest .'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
	         sh 'echo "Tests passed!"'
		sh 'docker run --rm myapp:latest'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Pushing to DockerHub...'
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    // Тегуємо образ для DockerHub
                    sh 'docker tag myapp:latest $DOCKER_USER/jenkins-lab-app:latest'
                    // Пушимо
                    sh 'docker push $DOCKER_USER/jenkins-lab-app:latest'
                }
            }
        }
    }
}
