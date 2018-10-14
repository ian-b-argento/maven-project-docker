pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.224.95.241', description: 'Staging Server')
		 string(name: 'tomcat_dev_hostkey', defaultValue: 'aa:ec:cb:f8:5d:f8:8b:03:b3:8c:73:ba:f4:65:91:39', description: 'AWS EC2 host key fingerprint for Staging')
         string(name: 'tomcat_prod', defaultValue: '18.224.214.188', description: 'Production Server')
		 string(name: 'tomcat_prod_hostkey', defaultValue: 'd6:ad:53:a1:69:a4:21:a8:d3:08:9c:a6:a3:1f:f6:48', description: 'AWS EC2 host key fingerprint for Staging')
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