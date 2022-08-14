pipeline {
    agent any

    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }
        stage("downloadgit"){
            steps{
            git 'https://github.com/vijayvikma/samplejenkinproject.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true install' 
            }
        }    
        stage('deploy') {
            steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'mydockerhostadmin', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''cd /opt/docker;
./deploy.sh''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//opt//docker', remoteDirectorySDF: false, removePrefix: 'webapp/target/', sourceFiles: 'webapp/target/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }    

    }
    post{
            success {
                junit 'target/surefire-reports/**/*.xml' 
            }
            failure{
                echo "failed"
            }
    }
}
