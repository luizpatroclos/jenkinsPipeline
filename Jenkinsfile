def applicationName = "jenkinspipeline";
def applicationNameST = "${applicationName}-st";

def OC_PASSWORD = "dev";
def OC_SERVER = "openshift.oc.techfirm.cloud:8443";
def OC_USER = "dev";
def OC_PROJECT_NAME = "jenkinspipeline";
def OC_PROJECT_DESCRIPTION = "Pipeline Test";

pipeline{
    agent any
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
                        oc login -u "${OC_USER}" -p "${OC_PASSWORD}" "${OC_SERVER}" && echo "Logged in as ${OC_USER} on Openshift ${OC_SERVER}"
                         """
                      echo 'Successfully'

                      echo '###################################'

                      echo 'delete openshift project for currently loaded environment'

                       //sh """
                       // oc delete project "${OC_PROJECT_NAME}"
                        //  """
                  }
                }
            }
            stage('Openshift'){

                    try {
                                sh """ oc project "${OC_PROJECT_NAME}" """
                            }
                            catch (exc) {
                                echo 'User do not exists'
                            }


                steps{
                  script{
                      echo 'login to openshift project for currently loaded environment'
                      sh """
                      oc login -u "${OC_USER}" -p "${OC_PASSWORD}" "${OC_SERVER}" && echo "Logged in as ${OC_USER} on Openshift ${OC_SERVER}"
                         """
                      echo 'Successfully'

                      echo '###################################'

                      echo 'delete openshift project for currently loaded environment'

                      sh """
                      oc delete project "${OC_PROJECT_NAME}"
                         """
                  }
                }
            }


            stage('Create Project Again'){
             when { not { OC_PROJECT_NAME 'master' } }
                steps{
                    script{
                    echo 'create openshift project for currently loaded environment (if not exists)'

                      sh """
                      oc new-project "${OC_PROJECT_NAME}" --description="${OC_PROJECT_DESCRIPTION}" || true
                      """
                      echo 'New Project Created'
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