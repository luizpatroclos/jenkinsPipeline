def applicationName = "jenkinspipeline";
def projectName = "conciliation";

pipeline{
    agent any

    parameters {

           string(name: 'OC_PASSWORD', defaultValue: 'dev', description: 'pass')
           string(name: 'OC_SERVER', defaultValue: 'openshift.oc.techfirm.cloud:8443', description: 'server')
           string(name: 'OC_USER', defaultValue: 'dev', description: 'user')
           string(name: 'OC_PROJECT_NAME', defaultValue: 'jenkinspipeline', description:'')
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
            stage('Openshift-buid'){

                steps{ //Step two create a new project in openshift
                    script {

                    //log in to openshift project for currently loaded environment
                    sh """
                     oc login -u ${params.OC_USER} -p ${params.OC_PASSWORD} ${params.OC_SERVER} && echo 'Logged in as ${params.OC_USER} on Openshift ${params.OC_SERVER}'

                     oc new-project ${params.OC_PROJECT_NAME} || oc project ${params.OC_PROJECT_NAME} \
                     &&  echo 'Try to create  as ${params.OC_USER} on Openshift the project  ${params.OC_PROJECT_NAME}'
                       """
                    }
                }
            }

            stage('Deploy') {
                 //deploy currently loaded environment with url given as 1st parameter
                 //will process and apply every yml template files in openshift folder
                 //with required environment parameters
              steps{
                script{
                  sh '''
                      fqdn="conciliation-${safebranch}.oc.techfirm.cloud"

                      for file in openshift/*.yml; do
                        oc process \
                          --ignore-unknown-parameters=true -f \${file} \
                          PROJECT_NAME=projectName \
                          VERSION="${safebranch}" \
                          REVISION="${GIT_COMMIT}" \
                          FQDN="${fqdn}" | oc apply -f -
                      done

                      for file in openshift/mocks/*.yml; do oc process --ignore-unknown-parameters=true -f \${file} \
                                  PROJECT_NAME=${params.OC_PROJECT_NAME} \
                                  VERSION="${safebranch}" \
                                  REVISION="${GIT_COMMIT}" \
                                  FQDN="${fqdn}" | oc apply -f -
                      done

                  '''
                }
              }
            }
    }
}