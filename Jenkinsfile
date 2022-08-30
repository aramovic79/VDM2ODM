def IS_STARTED_BY_USER = currentBuild.rawBuild.getCause(hudson.model.Cause$UserIdCause) != null

pipeline {
    agent any

    stages {
        stage('Init') {
            steps {
                checkout scm
            }
        }

        stage('Trigger VDM2ODMConverter Job') {
            when {
                // The stage is triggered if models directory was modified OR the pipeline was manually triggered.
                anyOf {
                    changeset 'models/**/*.json'
                    expression { return IS_STARTED_BY_USER }
                }
            }
            steps {
                script {
                    echo 'Some meaningfull message.'
                    // if (fileExists('models')) {
                    //     // Trigger the pipeline job that runs on VDMtoCDSConverter and creates a PR on ODM.
                    //     build job: 'VDM2ODMConverter', parameters: [
                    //         string(name: 'UPSTREAM_GIT_REPOSITORY', value: env.GIT_URL),
                    //         string(name: 'UPSTREAM_BUILD_URL', value: env.BUILD_URL),
                    //         string(name: 'UPSTREAM_GIT_COMMIT', value: env.GIT_COMMIT)
                    //     ]
                    // } else {
                    //     echo 'No model changes. VDM2ODMConverter not triggered.'
                    //     currentBuild.result = 'ABORTED'
                    // }
                }
            }
        }
    }
}
