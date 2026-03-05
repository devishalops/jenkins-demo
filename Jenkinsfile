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
                    /var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/sonar-scanner/bin/sonar-scanner \
                    -Dsonar.projectKey=jenkins-demo \
                    -Dsonar.sources=.
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t jenkins-demo .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 8082:80 jenkins-demo'
            }
        }

    }
}
