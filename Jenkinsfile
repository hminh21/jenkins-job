pipeline {
  agent any
 triggers {
    GenericTrigger(
     genericVariables: [
      [key: 'action', value: '$.action'],
      [key: 'number', value: '$.number'],
      [key: 'repo', value: '$.repository.name']
     ],
     token: 'TriggerPR',
     causeString: 'Triggered on $action Pull Request',
     regexpFilterText: '$action',
     regexpFilterExpression: 'closed',
     printContributedVariables: true,
     printPostContent: true
    )
 }

 environment {
    PR_WORKSPACE_DIRECTORY = "${env.JENKINS_HOME}/jobs/kobiton/jobs/${repo}/branches/PR-${number}"
 }

  stages {
    stage('Removing PR-$number $action in workspace') {
        steps {
            script {
                if (repo == "booster-automated-execution-runner") {
                    PR_WORKSPACE_DIRECTORY = "${env.JENKINS_HOME}/jobs/kobiton/jobs/booster-auto.1vadu4.ution-runner/branches/PR-${number}"
                }

                if (fileExists(env.PR_WORKSPACE_DIRECTORY)) {
                    dir("${env.PR_WORKSPACE_DIRECTORY}")
                    {
                        //deleteDir()
                        echo "Removed at ${env.PR_WORKSPACE_DIRECTORY}"
                        //echo "PR-${env.number} in ${env.repo} has been cleaned"
                    }
                }
                else {
                    error("CLEANING FAIL BECAUSE PR DOES NOT EXIST IN WORKSPACE")
                }
            }
        }
    }
  }
}
