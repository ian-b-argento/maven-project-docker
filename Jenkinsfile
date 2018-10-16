pipeline {
    agent any

    parameters {
         string(name: 'hostkey_prefix1', defaultValue: 'ssh-ed25519', description: 'Prefix #1 for the hostkey')
		 string(name: 'hostkey_prefix2', defaultValue: '256', description: 'Prefix #2 for the hostkey')
		 string(name: 'hostkey_prefix3', defaultValue: 'ssh-rsa', description: 'Prefix #3 for the hostkey')
		 string(name: 'hostkey_prefix4', defaultValue: '2048', description: 'Prefix #4 for the hostkey')
		 string(name: 'tomcat_dev', defaultValue: '18.224.95.241', description: 'Staging Server')
		 string(name: 'tomcat_dev_hostkey', defaultValue: 'aa:ec:cb:f8:5d:f8:8b:03:b3:8c:73:ba:f4:65:91:39', description: 'AWS EC2 host key fingerprint for Staging')
         string(name: 'tomcat_prod', defaultValue: '18.224.214.188', description: 'Production Server')
		 string(name: 'tomcat_prod_hostkey', defaultValue: 'U7b3j2Hye6ZnZCKzSi61lprCm8s3FsMcxRu7SUIT82E', description: 'AWS EC2 host key fingerprint for Production')
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
                        bat "tomcat-deploy ${params.tomcat_dev} ${params.hostkey_prefix1} ${params.hostkey_prefix2} ${params.tomcat_dev_hostkey}"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "tomcat-deploy ${params.tomcat_prod} ${params.hostkey_prefix3} ${params.hostkey_prefix4} ${params.tomcat_prod_hostkey}"
                    }
                }
            }
        }
    }
}