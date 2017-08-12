void setBuildStatus(String message, String state) {
    step([
            $class: "GitHubCommitStatusSetter",
            reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/my-org/my-repo"],
            contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/build-status"],
            errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
            statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
    ]);
}

node {
    checkout scm
    stages {
        if (env.BRANCH_NAME == 'master') {
            stage ('Deploy') {
                sshagent('GitHub') {
                    try {
                        sh 'mvn release:prepare'
                        sh 'mvn release:perform'
                    } catch (e) {
                        sh 'mvn clean install'
                    } finally {
                        setBuildStatus("Build complete", "SUCCESS");
                    }
                }
            }
        }
    }
}