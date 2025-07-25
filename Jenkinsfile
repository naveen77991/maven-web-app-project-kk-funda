pipeline
{
    agent any
    tools
    {
     maven "maven-3.9.8"
    }
    stages{
        stage('one')
        {
            steps{
                git 'https://github.com/naveen77991/maven-web-app-project-kk-funda.git'
            }
        }
        stage('two')
        {
            steps{
                sh 'mvn clean package'
            }
        }
        stage('three') {
            steps{
                sh 'mvn clean sonar:sonar'
            }
        }
        stage('four') {
            steps{
                sh 'mvn clean deploy'
            }
        }
        stage('fifth'){
            steps{
                deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: '4dabfcaf-0f0c-49e8-8df5-f34035d70dfc', path: '', url: 'http://34.224.82.3:8080/')], contextPath: null, war: '**/*.war'
            }
        }
    }
    post {
        success {
            script {
                slackSend(
                    channel: '#naveen-jenkins',
                    color: 'good',
                    message: "✅ *Build SUCCESS*\n*Job:* ${env.JOB_NAME} [#${env.BUILD_NUMBER}]"
                )
            }
        }
        failure {
            script {
                slackSend(
                    channel: '#naveen-jenkins',
                    color: 'danger',
                    message: "❌ *Build FAILED*\n*Job:* ${env.JOB_NAME} [#${env.BUILD_NUMBER}]\nCheck console log!"
                )
            }
        }
    }

}
