// def applicationName = "openshift";
// def applicationNameST = "${applicationName}-st";
//
// pipeline {
//     agent any
//
//     stages {
//         stage('try connect openshift'){
//                       steps{
//                             echo 'Trying build project with OpenShift'
//                           script{
//                               openshift.withProject {
//                               // find "default" cluster configuration and fallback to OpenShift cluster detection
//                               // ... operations relative to the default cluster ...
//                                 echo 'return something'
//                                  echo "default project: ${openshift.project()}"
//                               }
//                           }
//                       }
//         }
//         stage('Test') {
//             steps {
//                 echo 'Testing..'
//             }
//         }
//         stage('Deploy') {
//             steps {
//                 echo 'Deploying....'
//             }
//         }
//     }
// }