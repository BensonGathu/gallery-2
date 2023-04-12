pipeline{
    agent any
    tools {nodejs "nodejs"}
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
                post { 
                failure {
                    echo "testing failed"
                    emailext body: 'A Test EMail', recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: 'Test Failed'

                    }
                success {
                    echo "testing success"
                        emailext body: 'A Test EMail', recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: 'Test Success'

                    }

            }  
        } 

        stage('Deploy to Heroku') {
            steps {
            withCredentials([usernameColonPassword(credentialsId: 'heroku', variable: 'HEROKU_CREDENTIALS' )]){
            sh 'git push https://${HEROKU_CREDENTIALS}@git.heroku.com/infinite-coast-81166.git master'
    }
  }
} 
    }
}

 