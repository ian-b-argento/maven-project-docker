pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.224.95.241', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.224.214.188', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
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
                stage ('Deploy to Staging'){
                    steps {
                        bat "winscp.com -command ""open ec2-user@${params.tomcat_dev} -privatekey=""""C:\Users\IanBradshaw\.ssh\tomcat-demo.ppk"""" ""put webapp\target*.war /var/lib/tomcat8/webapps"" ""exit"""
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "winscp.com -command ""open ec2-user@${params.tomcat_prod} -privatekey=""""C:\Users\IanBradshaw\.ssh\tomcat-demo.ppk"""" ""put webapp\target*.war /var/lib/tomcat8/webapps"" ""exit"""
                    }
                }
            }
        }
    }
}