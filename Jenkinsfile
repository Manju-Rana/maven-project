pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ("deploy to staging")
        {
            steps {
                build job: 'deploy-to-staging'
            }
        }
       stage ("deploy to production")
        {
            steps {
                    timeout(time:5, unit: 'DAYS')
                    {
                        input message:  'approve Prod Deployment ?'                     
                    }
                
                build job: 'deploy-to-prod'
            }

            post {
                    success {
                        echo 'Deployed to Prod. Yahoo!!!'
                    } 
                    failure {
                        echo 'its failed'
                    }

            }
        }

    }
}
