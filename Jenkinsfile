pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo " Running build stage..."
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts : 'dist/trainSchedule.zip'
            }
        }
        stage('Pulish to StagingServer'){
            when {
                branch 'master'
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
            
                sshPublisher(
                    failOnError: true,
                    publishers: [
                    sshPublisherDesc(configName: 'staging', 
                                     sshCredentials: [
                                        username: "$USERNAME",
                                        encryptedPassphrase: "$USERPASS"
                                     ],
                                    transfers: [
                                        sshTransfer(
                                            
                                            execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule', 
                                            execTimeout: 120000, 
                                            flatten: false, 
                                            makeEmptyDirs: false, 
                                            noDefaultExcludes: false, 
                                            patternSeparator: '[, ]+', 
                                            remoteDirectory: '/tmp', 
                                            remoteDirectorySDF: false, 
                                            removePrefix: 'dist/', 
                                            sourceFiles: 'dist/trainSchedule.zip')], 
                                            usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
     }
  }
}
