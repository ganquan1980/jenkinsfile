pipeline{
    agent{
        node{
            label '192.168.8.184'
            }
        }
    tools{maven 'apache-maven-3.3.9'} 
    stages{
        stage('checkout from svn'){
            steps{
                checkout([$class: 'SubversionSCM', additionalCredentials: [], excludedCommitMessages: '', excludedRegions: '', excludedRevprop: '', excludedUsers: '', filterChangelog: false, ignoreDirPropChanges: false, includedRegions: '', locations: [[credentialsId: 'd055b494-b933-4f4a-827f-49170f58a164', depthOption: 'infinity', ignoreExternalsOption: true, local: '.', remote: 'http://192.168.133.163/GXB/ZBM/zyxbV2/project/gbp-swb/g2-swb']], workspaceUpdater: [$class: 'UpdateUpdater']])
            }
        }
        stage('handle config file'){
            steps{
                sh '''
                rm -rf ./src/main/webapp/WEB-INF/classes/*
                cp -R ./src/main/resources/* ./src/main/webapp/WEB-INF/classes/
                cp -R ./src/main/java/* ./src/main/webapp/WEB-INF/classes/
                sed -i \'s/host=.*/host=192.168.8.184/g\' ./src/main/webapp/WEB-INF/config/redis.properties
                sleep 1'''

            }
        }
        stage('Exec Maven'){
            steps{
                sh '''mvn clean install'''
            }
        }
         stage('replace file and restart tomcat'){
            steps{
               sh '''project_name=llh-swb-tomcat
                     webapp_name=main
                     pid=`ps -ef|grep java|grep llh|gawk '$0!~/grep/ {print $2}'|tr -s '\n'''`
                     echo $pid
                    if [ "$pid" ]; then ps -ef | grep llh| grep -v grep | cut -c 9-15 | xargs kill -s 9; fi
                    sleep 2s
                    rm -rf /home/llh-swb-tomcat/webapps/zfcg
                    rm -rf /home/llh-swb-tomcat/webapps/zfcg/zfcg.war
                    mv ./target/gbp-swb.war ./target/zfcg.war
                    cp ./target/zfcg.war /home/llh-swb-tomcat/webapps/
            
                    JENKINS_NODE_COOKIE=dontKillMe
                    nohup /home/llh-swb-tomcat/bin/startup.sh&
                    sleep 10s
                    unset pid
                    '''
            }
        }
        stage('send email'){
            steps{
               mail bcc: '', body: '''商务部项目系统自动部署完成
                部署版本号Build # $BUILD_NUMBER
                部署地址为:https://
                部署状态为:$BUILD_STATUS''', cc: '', from: 'xajq@163.com', replyTo: '', subject: '商务部项目自动部署', to: 'ganq@glodon.com'
            }
        }
    }
}
