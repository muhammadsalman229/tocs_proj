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
                    } else if (env.BRANCH_NAME == 'feature_2') {
                        remoteDirectory = '/var/www/html/feature_2'
                    }
                    
                    sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: 'ApacheServer',
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: '*/.html, */.css, */images//',
                                        remoteDirectory: remoteDirectory,
                                        removePrefix: '',
                                        execCommand: ''
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