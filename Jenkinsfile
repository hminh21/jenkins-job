pipeline {
  agent { 
      label any
    }
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
    PR_WORKSPACE_DIRECTORY = "${env.JENKINS_HOME}/jobs/kobiton-inc/jobs/${env.repo}/branches/PR-${env.number}"
 }

  stages {
    stage('Removing PR-$number $action in workspace') {
        steps {
            script {
                if (env.repo == "booster-automated-execution-runner") {
                    PR_WORKSPACE_DIRECTORY = "${env.JENKINS_HOME}/jobs/kobiton/jobs/booster-auto.1vadu4.ution-runner/branches/PR-${env.number}"
                }

                if (fileExists(env.PR_WORKSPACE_DIRECTORY)) {
                    dir("${env.PR_WORKSPACE_DIRECTORY}")
                    {
                        deleteDir()
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
  
  post {
      success {
          script {
              withCredentials([string(credentialsId: '0f35f85f-d840-4caa-b49c-0ed13605a301', variable: 'API_TOKEN')]) {
             //Get Jenkins-Crumb
             resJson = sh(script: "curl -s -u hminh21:${API_TOKEN} https://665d4b32.ngrok.io/crumbIssuer/api/json", returnStdout: true)
             def res = readJSON text: resJson
             
             //Do delete job in jenkins
             statusCode = sh(script: "curl -f --show-error -s -X POST -u hminh21:${API_TOKEN} -H 'Jenkins-Crumb:${res['crumb']}' https://a27a5fef.ngrok.io/job/kobiton-inc/job/${env.repo}/view/change-requests/job/PR-${env.number}/doDelete", returnStatus: true)

             if (statusCode == 0) {
                 echo "PR-${env.number} job in ${env.repo} has been deleted"
                 }
             else {
                 echo "REMOVING JOB FAIL BECAUSE OF SENDING TO REST API JENKINS FAIL"
                 currentBuild.result = "FAILURE"
                }
              }
            }
        }
    }
}
