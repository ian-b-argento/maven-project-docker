pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.224.95.241', description: 'Staging Server')
		 string(name: 'tomcat_dev_hostkey', defaultValue: 'ssh-ed25519 256 pDeGMWohclzaC+jj0qKMkjiS78dc7AnyY4EQ7O+aDzE=', description: 'AWS EC2 host key fingerprint for Staging')
         string(name: 'tomcat_prod', defaultValue: '18.224.214.188', description: 'Production Server')
		 string(name: 'tomcat_prod_hostkey', defaultValue: 'ssh-ed25519 256 uTXzb3wdfK2qMEL2uvyENbnV+KgErSIuJhYQ5EgRmeU=', description: 'AWS EC2 host key fingerprint for Production')
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
                        bat "tomcat-deploy ${params.tomcat_dev} ${params.tomcat_dev_hostkey}"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "tomcat-deploy ${params.tomcat_prod} ${params.tomcat_prod_hostkey}"
                    }
                }
            }
        }
    }
}