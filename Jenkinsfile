pipeline {
    agent any
    stages{
        stage('clone'){
            steps{
                checkout scmGit(branches: [[name: '*/DEV']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/NagarajuSamboju/raviLogin.git']])
            }
        }
        stage('build with MVN'){
            steps{
                sh 'mvn package'
            }
        }
        stage('Pull Request Approval') {
           when {
            expression {
                currentBuild.resultIsBetterOrEqualTo('SUCCESS') &&
                currentBuild.changeSets.any { entry ->
                entry.kind == 'NORMAL' && entry.authorEmail.endsWith('BMATHEEN96@GMAIL.COM')
            }
        }
    }
    steps {
        script {
            def slackMessage = "Approval needed for deployment to Test environment.\n" +
                               "Please review and approve this pull request."
            
            // Use a Slack integration token or webhook to send the message
            // Replace 'YOUR_SLACK_TOKEN_OR_WEBHOOK_URL' with your actual Slack token or URL
            sh "curl -X POST -H 'Content-type: application/json' --data '{\"text\":\"${slackMessage}\"}' http://35.175.128.196:8080/github-webhook/"
        }
    }
}
        stage('tomcat'){
            steps{
                deploy adapters: [tomcat9(credentialsId: 'tomcat1', path: '', url: 'http://100.26.239.79:8080/')], contextPath: 'Taxi-booking', war: '**/*.war'
            }
        }
    }
}
