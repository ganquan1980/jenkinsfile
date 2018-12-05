pipeline {
    agent none
    stages{
        stage('checkout'){
            steps{
                checkout([$class: 'SubversionSCM', additionalCredentials: [], excludedCommitMessages: '', excludedRegions: '', excludedRevprop: '', excludedUsers: '', filterChangelog: false, ignoreDirPropChanges: false, includedRegions: '', locations: [[credentialsId: 'd055b494-b933-4f4a-827f-49170f58a164', depthOption: 'infinity', ignoreExternalsOption: true, local: '.', remote: 'http://192.168.133.163:80/GXB/ZBM/zyxbV2/project/gbp-swb/g2-swb']], workspaceUpdater: [$class: 'UpdateUpdater']])
            }
        }
    }
}