pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '3107.23.235.120', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '34.209.233.6', description: 'Production Server')
    }

    triggers {
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
                stage ('Deploy to Staging'){
                    steps {
                        sh "sshpass -p 'redhat' scp **/target/*.war root@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                    }
                }

                
            }
        }
    }
}


