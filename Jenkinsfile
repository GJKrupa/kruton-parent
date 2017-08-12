pipeline {
    stages {
        if (env.BRANCH_NAME == 'master') {
            stage ('Deploy') {
                sshagent('GItHub') {
                    try {
                        sh 'mvn release:prepare'
                        sh 'mvn release:perform'
                    } catch (e) {
                        sh 'mvn clean install'
                    }
                }
            }
        }
    }
}