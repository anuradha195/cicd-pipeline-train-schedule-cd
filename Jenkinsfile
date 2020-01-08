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
        stage('Pulish to StagingServer'){
            when {
                branch 'master'
            }
            sshPublisher(publishers: [sshPublisherDesc(configName: 'staging', sshCredentials: [encryptedPassphrase: '{AQAAABAAAAAQOL2rnKHVkKsV9STcUN9Cn5hKenR7aqfE4i3N66y0Uf0=}', key: '', keyPath: '', username: 'deploy'], transfers: [sshTransfer(excludes: '', execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/tmp', remoteDirectorySDF: false, removePrefix: 'dist/', sourceFiles: 'dist/trainSchedule.zip')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])

        }
        

    }
}
