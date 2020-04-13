pipeline {
    agent any

    
 environment {
        //be sure to replace "willbla" with your own Docker Hub username
        DOCKER_IMAGE_NAME = "sw3:5000/hello-test"
    }





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
                withCredentials([usernamePassword(credentialsId: 'ssh-user3', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'user2',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ],
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'target/*.jar',
                                        removePrefix: 'target',
                                        remoteDirectory: '/home/user2/swjar/',
                                        execCommand: 'echo hello'
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }



 stage('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    app = docker.build("sw3:5000/hello-test")
                }
            }
        }



stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('http://sw3:5000', 'pvt-docker') {
                    app.push()
                    }
                }
            }
        }




 stage('DeployToProduction') {
            when {
                branch 'master'
            }

      steps {
                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig',
                    configs: 'hellotest-kube.yml',
                    enableConfigSubstitution: true
                )
            }


        }










 }
}
