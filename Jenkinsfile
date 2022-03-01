pipeline { 
    agent any  
    stages { 
        stage('SCM from Mirror') { 
            steps { 
               git url: 'http://172.29.4.49/218352001_njtech/backend-projectname.git'
               sh "ls -al"
            }
        }
        stage('Build') { 
            steps { 
               sh "chmod +x mvnw"
               sh "./mvnw install"
               junit 'target/surefire-reports/*.xml'
               step( [ $class: 'JacocoPublisher' ] )
            }
            post {
                failure {
                    updateGitlabCommitStatus name: 'build', state: 'failed'
                }
                success {
                    updateGitlabCommitStatus name:'build', state: 'success'
                }
            }
        }
        stage('Launch') {
            steps { 
               sh "/usr/local/shell/start_collect_backend.sh"
            }
        }
    }
}
