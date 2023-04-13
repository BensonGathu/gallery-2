pipeline{
    agent any
    tools {nodejs "nodejs"}
     environment {

        EMAIL_BODY = 

        """

            <p>EXECUTED: Job <b>\'${env.JOB_NAME}:${env.BUILD_NUMBER})\'</b></p>

            <p>

            View console output at 

            "<a href="${env.BUILD_URL}">${env.JOB_NAME}:${env.BUILD_NUMBER}</a>"

            </p> 

            <p><i>(Build log is attached.)</i></p>

        """

        EMAIL_SUBJECT_SUCCESS = "Status: 'SUCCESS' -Job \'${env.JOB_NAME}:${env.BUILD_NUMBER}\'" 

        EMAIL_SUBJECT_FAILURE = "Status: 'FAILURE' -Job \'${env.JOB_NAME}:${env.BUILD_NUMBER}\'" 

        EMAIL_RECEPIENT = 'gathubenson23@gmail.com'

    }
    stages{
        stage("Install Packages"){
            steps{
                echo 'Installing packages'
                sh "npm install"
            }
        } 
        stage ("testing"){ 
            steps{
                echo "testing application"
                sh "npm test"}
            //     post { 
            //     failure {
            //         echo "testing failed"
            //         emailext body: 'A Test EMail', recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: 'Test Failed'

            //         }
            //     success {
            //         echo "testing success"
            //             emailext body: 'A Test EMail', recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: 'Test Success'

            //         }

            // }  

                post {
        success {
            emailext attachLog: true, 
                body: EMAIL_BODY, 

                subject: EMAIL_SUBJECT_SUCCESS,

                to: EMAIL_RECEPIENT
        }

        failure {
            emailext attachLog: true, 
                body: EMAIL_BODY, 

                subject: EMAIL_SUBJECT_FAILURE, 

                to: EMAIL_RECEPIENT
        }
    }}

        stage('Deploy to Heroku') {
            steps {
            withCredentials([usernameColonPassword(credentialsId: 'heroku', variable: 'HEROKU_CREDENTIALS' )]){
            sh 'git push https://${HEROKU_CREDENTIALS}@git.heroku.com/infinite-coast-81166.git master'
    }
  }
} 
    }
}

 