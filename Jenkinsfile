pipeline {
    agent any

    tools {
        maven "maven-3.9.8"
    }

    stages {
        stage('clone') {
            steps {
                git branch: 'dev', url: 'https://github.com/naveen77991/maven-web-app-project-kk-funda.git'
            }
        }

        stage('build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('sonar') {
            steps {
                sh 'mvn clean package sonar:sonar'
            }
        }

        stage('articat') {
            steps {
                sh 'mvn deploy sonar:sonar'
            }
        }

        stage('tomcat') {
            steps {
                echo "Deploying WAR file using curl..."
                sh """
                    curl -u kkfunda:kkfunda \
                    --upload-file /var/lib/jenkins/workspace/jenkins-declarative/target/maven-web-application.war \
                    "http://54.167.64.22:8080/manager/text/deploy?path=/maven-web-application&update=true"
                """
            }
        }
    }

    post {
        success {
            script {
                slackSend(
                    channel: '#jenkins-slack',
                    color: 'good',
                    message: "✅ *Build SUCCESS*\n*Job:* ${env.JOB_NAME} [#${env.BUILD_NUMBER}]"
                )
            }
        }
        failure {
            script {
                slackSend(
                    channel: '#jenkins-slack',
                    color: 'danger',
                    message: "❌ *Build FAILED*\n*Job:* ${env.JOB_NAME} [#${env.BUILD_NUMBER}]\nCheck console log!"
                )
            }
        }
    }
}

