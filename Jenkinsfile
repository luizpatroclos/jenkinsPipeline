def applicationName = "openshift";
def applicationNameST = "${applicationName}-st";

pipeline{
    openshift.withCluster() { // Use "default" cluster or fallback to OpenShift cluster detection
        echo "Hello from the project running Jenkins: ${openshift.project()}"
    }
}