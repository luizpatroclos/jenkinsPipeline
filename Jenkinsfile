def applicationName = "openshift";
def applicationNameST = "${applicationName}-st";

pipeline {
    agent any
    stages {

      stage ('buildInDevelopment') {

        steps{
          echo 'Trying build project with OpenShift'

           script{

             openshift.withCluster( 'testpipeline', 'jenkins_openshift' ) {

             echo 'Autenticated'
              }

              openshift.withCluster( 'testpipeline' ) {
                  openshift.doAs( 'jenkins_openshift' ) {

                    echo 'Second Possibility'
                  }
              }
           }

           openshift.withProject {
                // find "default" cluster configuration and fallback to OpenShift cluster detection
                // ... operations relative to the default cluster ...
                echo 'return something'
                echo "default project: ${openshift.project()}"
           }
        }
      }


       stage ('buildInDevelopment2') {

            steps{
               openshiftBuild(namespace: 'jenkinspipeline', bldCfg: 'jenkinspipeline', showBuildLogs: 'true')
            }
       }

          stage ('deployInDevelopment') {

              steps{

                 openshiftDeploy(namespace: 'jenkinspipeline', depCfg: 'jenkinspipeline')

              }
          }

              stage('try connect openshift'){
                      steps{
                            echo 'Trying build project with OpenShift'
                          script{
                              openshift.withProject {
                              // find "default" cluster configuration and fallback to OpenShift cluster detection
                              // ... operations relative to the default cluster ...
                                echo 'return something'
                                 echo "default project: ${openshift.project()}"
                              }
                          }
                      }
              }
    }
}
