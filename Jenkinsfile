def applicationName = "openshift";
def applicationNameST = "${applicationName}-st";

pipeline {
    agent any

    stages {
        stage('try connect openshift'){
                      steps{
                            echo 'Trying build project with OpenShift'
                          script{
                              openshift.withCluster() { // Use "default" cluster or fallback to OpenShift cluster detection
                                      echo "Hello from the project running Jenkins: ${openshift.project()}"
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