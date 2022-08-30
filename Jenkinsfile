
pipeline {
    agent any

    stages {
        stage('Init') {
            steps {
                checkout scm
            }
        }

        stage('Trigger VDM2ODM Job') {
            when {
                // The stage is triggered if models directory was modified OR the pipeline was manually triggered.
                anyOf {
                    changeset 'models/**/*.json'
                }
            }
            steps {
                script {
                    if (fileExists('models')) {
                        // Trigger the pipeline job that runs on VDMtoCDSConverter and creates a PR on ODM.
                        build job: 'VDM2ODM', parameters: [
                            string(name: 'UPSTREAM_GIT_REPOSITORY', value: env.GIT_URL),
                            string(name: 'UPSTREAM_BUILD_URL', value: env.BUILD_URL),
                            string(name: 'UPSTREAM_GIT_COMMIT', value: env.GIT_COMMIT)
                        ]
                    } else {
                        echo 'No model changes. VDM2ODM not triggered.'
                        currentBuild.result = 'ABORTED'
                    }
                }
            }
        }
    }
}
