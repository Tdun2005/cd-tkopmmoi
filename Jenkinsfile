pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/Tdun2005/cd-tkopmmoi.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Build success'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploy done'
            }
        }
    }
}

