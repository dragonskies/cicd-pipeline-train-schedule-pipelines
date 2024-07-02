pipeline {
  agent any
  stages {
    stage ('Build') {
      steps {
        echo "Running build automation"
        sh './gradlew build --no-daemon'
        archiveArtifacts artifacts: 'dist/trainSchedule.zip'
      }
    }
    stage ('DeployToStaging'){
           when {
             branch 'master'
           }
      steps {
        withCredentials([usernamePassword(credentialsId: '635a24fb-956d-4148-8790-8d54ffe85e62', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]{
            sshPublisher(
              failOnError: true,
              continueOnError: false,
              publishers: [ sshPublisherDesc(
                configName: 'staging',
                sshCredentials: [
                  username: "USERNAME",
                  encryptedPasspharse: "USERPASS"
                  ]
                transfers: [
                  sshTransfer(
                    sourceFiles: 'dist/trainSchedule.zip',
                    removePrefix: 'dist/',
                    remoteDirectory: '/tmp',
                    execCommand: 'sudo /usr/bin/sysemtl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo systemctl start train-schedule'
  
          }
``    `}  
    }
  }
 }
}
