pipeline {
    agent any
    triggers {
    GenericTrigger(
        genericVariables: [
            [defaultValue: '', key: 'base', regexpFilter: '', value: '$.ref'],
            ],
     causeString: 'Triggered on $ref',
     token: 'my-webhook-token',
     tokenCredentialId: '' )
  }
    stages {
        stage('Check branch name') {
            steps {
                script {
                    if (env.tagetBranchName != 'dev') {
                        echo "Branch name is not 'dev', proceeding with the merge"
                    } else {
                        error "Merge rejected! Branch name 'dev' is not allowed."
                    }
                }
            }
        }
        stage('Merge to main') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'larina', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                        git branch: 'dev', url: 'https://github.com/leexien/jenkins-dubl1.git'
                        git commit: 'Merging dev into main', url: 'https://github.com/leexien/jenkins-dubl1.git'
                        git mergeRemote: 'origin', remote: env.sourseBranchName
                    }
                }
            }
        }
    }
}
