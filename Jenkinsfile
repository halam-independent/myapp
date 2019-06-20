pipeline {
    agent any
    stages {
        stage ('Build') {
            steps{
                cd /home/tests/myapp
                npm install
                npm start
            }
        }
        stage ('DeployToStaging') {
            when {
                branch 'master'
            }
            steps{
                 withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'staging',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ]
                            )
                        ]
                    )
                }
            }
        }
        stage ('DeployToProduction') {
            when {
                branch 'master'
            }
            steps{
                input 'Does the staging environment look OK?'
                milestone(1)
                withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'production',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ]
                            )
                        ]
                    )
                }
            }
        }
    }
}
