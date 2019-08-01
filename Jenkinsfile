def applicationName = "jenkinspipeline";
def applicationNameST = "${applicationName}-st";

pipeline{
    agent any

    parameters {

           string(name: 'OC_PASSWORD', defaultValue: 'dev', description: 'pass')
           string(name: 'OC_SERVER', defaultValue: 'openshift.oc.techfirm.cloud:8443', description: 'server')
           string(name: 'OC_USER', defaultValue: 'dev', description: 'user')
           string(name: 'OC_PROJECT_NAME', defaultValue: 'jenkinspipeline', description:'')
           string(name: 'OC_PROJECT_DESCRIPTION', defaultValue: 'Pipeline Test', description: 'description')
           string(name: 'projetc', defaultValue: '')


     }

    stages{
            stage('build') {
                steps{
                    echo 'step 1'
                    //sh script: "cd ${applicationName} && mvn -DskipTests clean package"
                }
            }
            stage('build system tests') {
                steps{
                    echo 'step 2'
                    //sh script: "cd ${applicationNameST} && mvn clean package"
                }
            }
            stage('unit tests') {
                steps{
                    echo 'step 3'
                    //sh script: "cd ${applicationName} && mvn test"
                }
            }
            stage('integration tests') {
                steps{
                    echo 'step 4'
                    //sh script: "cd ${applicationName} && mvn failsafe:integration-test failsafe:verify"
                }
            }
            stage('Openshift'){
                steps{
                  script{
                      echo 'login to openshift project for currently loaded environment'
                      sh """
                        oc login -u ${params.OC_USER} -p ${params.OC_PASSWORD} ${params.OC_SERVER} && echo 'Logged in as ${params.OC_USER} on Openshift ${params.OC_SERVER}'

                         """
                      echo 'Successfully'

                      echo 'delete openshift project for currently loaded environment'

                           sh  """
                                {
                                   oc delete project ${params.OC_PROJECT_NAME} &&  echo 'Try to delete  as ${params.OC_USER} on Openshift the project  ${params.OC_SERVER}'
                                } || {
                                   echo 'There is no project to delete'
                                }
                               """
                      echo 'Project has been deleted'

                       sh """
                         oc logout
                         """
                       echo ' User Logout'

                  }
                }
            }
            stage('Openshift-try project'){

                steps{
                    script {

                    echo 'login to openshift project for currently loaded environment'
                    sh """
                     oc login -u ${params.OC_USER} -p ${params.OC_PASSWORD} ${params.OC_SERVER} && echo 'Logged in as ${params.OC_USER} on Openshift ${params.OC_SERVER}'

                     oc new-project ${params.OC_PROJECT_NAME} &&  echo 'Try to create  as ${params.OC_USER} on Openshift the project  ${params.OC_PROJECT_NAME}'

                       """
                    }
                }
            }

            stage('verify service connectivity'){
                steps{
                    script{
                        echo 'step 4.6'
                    }
                }
            }
            stage('system tests') {
                steps{
                    echo 'step 1'
                    //sh script: "cd ${applicationNameST} && mvn failsafe:integration-test failsafe:verify"
                }
            }
    }
}