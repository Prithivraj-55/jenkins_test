properties([pipelineTriggers([githubPush()])])
pipeline {
    agent any
    
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    stages {
        stage("print1") {
            steps {
                echo "printing 1"
            }
        }
        stage("print2") {
            steps {
                echo "printing 2"
            }
        }
        stage('Build') {
            agent {
                label 'linux'
            }
            steps {
                // Get some code from a GitHub repository
                url: git 'https://github.com/Prithivraj-55/jenkins_test'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true -f api-gateway clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit 'api-gateway/target/surefire-reports/*.xml'
                    archiveArtifacts 'api-gateway/target/*.jar'
                    emailext body: "Please check the console output at $BUILD_URL for more information", to: "prithivrajthangadurai@gmail.com", subject: '$PROJECT_NAME is completed - Build number is $BUILD_NUMBER - Build status is $BUILD_STATUS'
                }
            }
        }
   
        stage("Email") {
            steps {
                script {
                    cest = TimeZone.getTimeZone("CEST")
                    def cest = new Date ()
                    println(cest)
                    def mailRecipients = 'prithivrajthangadurai@gmail.com'
                    def jobName = currentBuild.fullDisplayName
                    env.Name = Name
                    env.cest = cest
                    emailext body: '''${SCRIPT, template="email-html.template"}''', mimeType: 'text/html', subject: "[jenkins] ${jobName}", to: "${mailRecipients}", replyTo: "${mailRecipients}"
                }         
            }
        }
    }
}
