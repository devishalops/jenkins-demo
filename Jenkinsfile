pipeline {
agent any

stages {

    stage('Checkout Code') {
        steps {
            git branch: 'main', url: 'https://github.com/devishalops/jenkins-demo.git'
        }
    }

    stage('SonarQube Analysis') {
        steps {
            withSonarQubeEnv('sonarqube') {
                sh '''
                /opt/sonar-scanner/bin/sonar-scanner \
                -Dsonar.projectKey=jenkins-demo \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://localhost:9000
                '''
            }
        }
    }

    stage('Build Docker Image') {
        steps {
            sh 'docker build -t jenkins-demo .'
        }
    }

    stage('Stop Old Container') {
        steps {
            sh 'docker rm -f jenkins-demo-container || true'
        }
    }

    stage('Run Container') {
        steps {
            sh 'docker run -d -p 8082:80 --name jenkins-demo-container jenkins-demo'
        }
    }

}

post {
    success {
        echo 'Pipeline executed successfully!'
    }
    failure {
        echo 'Pipeline failed!'
    }
}

}
