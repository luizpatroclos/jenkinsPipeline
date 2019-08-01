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
           string(name: 'REPLICA_COUNT', defaultValue: '2')

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

                steps{ //Step one log in to openshift and delete the current project
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

                  }
                }
            }
            stage('Openshift-try project'){

                steps{ //Step two create a new project in openshift
                    script {

                    //log in to openshift project for currently loaded environment
                    sh """
                     oc new-project ${params.OC_PROJECT_NAME} || oc project ${params.OC_PROJECT_NAME} \
                     &&  echo 'Try to create  as ${params.OC_USER} on Openshift the project  ${params.OC_PROJECT_NAME}'
                       """
                    }
                }
            }

            stage('Return the current number of replicas'){
                steps{
                    script{

                        //Return the current number of replicas for a given 'app' label if it is an integer greater than 0, otherwise the environment configured replica count

                          sh '''
                            local current=$(oc get --ignore-not-found=true --no-headers=true -l app=\${1} dc | awk { print \$3 })
                            if [[ $current -gt 0 ]]
                            then
                              echo $current
                            else
                              echo ${params.REPLICA_COUNT}
                            fi
                          '''

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