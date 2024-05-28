pipeline {
    agent any
    stages {
        stage('Deploy') {
            steps {
                script {
                    def remoteDirectory = ""
                    if (env.BRANCH_NAME == 'main') {
                        remoteDirectory = '/var/www/html'
                    } else if (env.BRANCH_NAME == 'feature_1') {
                        remoteDirectory = '/var/www/html/feature_1'
                    }
                    
                    // Add the remote server to known hosts to avoid manual SSH key acceptance
                    sh 'ssh-keyscan -H 16.16.170.170 >> ~/.ssh/known_hosts'

                    // Deploy the index.html file
                    sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: 'ApacheServer',
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'index.html',  // Only transfer index.html
                                        remoteDirectory: remoteDirectory,
                                        removePrefix: '',
                                        execCommand: '''
                                        sudo chown -R www-data:www-data /var/www/html
                                        sudo chmod -R 755 /var/www/html
                                        '''
                                    )
                                ],
                                usePromotionTimestamp: false,
                                useWorkspaceInPromotion: false,
                                verbose: true
                            )
                        ]
                    )
                }
            }
        }
    }
}
