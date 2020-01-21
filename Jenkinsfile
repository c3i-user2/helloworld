pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
               echo "hello"  
               echo 'Running build automation'
                sh './mvnw install'
            }
        }


stage('DeployToStaging') {
            when {
                branch 'master'
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'ssh-user1', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'user1',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ],
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'target/*.jar',
                                        removePrefix: 'target',
                                        remoteDirectory: '/home/user1/swjar/',
                                        execCommand: 'echo hello'
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }



}
}
