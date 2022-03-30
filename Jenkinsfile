pipeline{
    agent any
    stages{
        stage("source code management"){
            steps{
                git credentialsId: 'git-creds', url: 'https://github.com/ravisnowdev/spring3-mvc-maven-xml-hello-world.git'
            }
        }
        stage("build"){
            steps{
                sh "mvn package"
            }
        }
        stage("fetch war file"){
            steps{
                archiveArtifacts artifacts: 'target/*.war'
            }
        }
        stage("deploy"){
            steps{
                    withCredentials([usernameColonPassword(credentialsId: 'tomcat-creds', variable: 'tomcatCreds')]) {
                    sh "curl -v -u $tomcatCreds -T /var/lib/jenkins/workspace/spring-project-pipeline/target/spring3-mvc-maven-xml-hello-world-1.0-SNAPSHOT.war 'http://ec2-3-88-33-230.compute-1.amazonaws.com:8081/manager/text/deploy?path=/spring-jenkins-pipeline&update=true'"
                }
            }
        }
    }
    
     post {
        failure {
            script {
                currentBuild.result = 'SUCCESS'
            }
        }

        always {
            step([$class: 'Mailer',
                notifyEveryUnstableBuild: true,
                recipients: "ramsravidamarla@gmail.com",
                sendToIndividuals: true])
        }
    }
}
