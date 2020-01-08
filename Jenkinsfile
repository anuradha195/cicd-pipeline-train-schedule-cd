pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo " Running build stage..."
                sh './gradlew build --no-daemon'
                archiveArtifacts : 'dist/trainSchedule.zip'
            }
        }
        stage('StagingServerDeployment') {
            when {
                branch 'master'
            }
            steps{
                sshPublisher(
                    publishers: [
                        sshPublisherDesc(
                            configName: 'staging', 
                            sshCredentials: [
                                encryptedPassphrase: '{AQAAABAAAAAQ6w5zVo4MqRLsM1eRwQ4LqWkad7Jn/2uLrMPMeK8N4hI=}', key: '', keyPath: '', username: 'cloud_user'
                                ], 
                                transfers: [
                                    sshTransfer(
                                        
                                        execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule',
                                        execTimeout: 120000, 
                                        flatten: false, 
                                        makeEmptyDirs: false, 
                                        noDefaultExcludes: false, 
                                        remoteDirectory: '/tmp', 
                                        remoteDirectorySDF: false, 
                                        removePrefix: 'dist/', 
                                        sourceFiles: 'dist/trainSchedule.zip'
                                        )
                                ], 
                                usePromotionTimestamp: false, 
                                useWorkspaceInPromotion: false, 
                                verbose: false)])


            }

        }
    }
}
