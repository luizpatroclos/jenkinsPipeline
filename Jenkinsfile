def applicationName = "openshift";
def applicationNameST = "${applicationName}-st";

pipeline {
    agent any

    stages {
        stage('try connect openshift'){
                      steps{
                            echo 'Trying build project with OpenShift'
                          script{
                              openshift.withCluster( 'OpenShift_PT', 'techfirm-openshift-dev' ) {
                                      echo "Hello from ${openshift.cluster()}'s default project: ${openshift.project()}"
                              }
                          }
                      }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}