pipeline {    
    agent any

    parameters {
            string(name: 'tomcat_stage', defaultValue:'3.16.187.113', description: 'Stage')
            string(name: 'tomcat_prod', defaultValue:'18.220.65.170', description: 'Prod')

    }

    triggers{
        pollSCM('* * * * *')
    }

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
    
        stage ('Deployments'){
            parallel{        
                stage ("deploy to staging")
                {
                    steps {
                        sh "sudo scp -i /Users/Shared/Jenkins/tomcatdemo.pem **/target/*.war ec2-user@${params.tomcat_stage}:/var/lib/tomcat7/webapps"
                    }
                }
               stage ("deploy to production")
                {
                    steps {
                            sh "sudo scp -i /Users/Shared/Jenkins/tomcatdemo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
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
 }
}
