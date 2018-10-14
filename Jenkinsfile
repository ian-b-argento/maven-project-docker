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
                        bat "tomcat-deploy ${params.tomcat_dev}"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "tomcat-deploy ${params.tomcat_prod}"
                    }
                }
            }
        }
    }
}