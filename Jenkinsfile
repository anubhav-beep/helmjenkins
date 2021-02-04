node('master'){
    
    def servicePrincipalId = 'azureserviceprincipal'
    def resourceGroup = 'jenkinsHelm'
    def aks = 'jenkinscluster'
    
    stage('SCM') {
        checkout scm
    }

    stage('Build') {
        withCredentials([azureServicePrincipal(servicePrincipalId)]) {
            sh """
                az login --service-principal -u "\$AZURE_CLIENT_ID" -p "\$AZURE_CLIENT_SECRET" -t "\$AZURE_TENANT_ID"
                az account set --subscription "\$AZURE_SUBSCRIPTION_ID"
                sh ./mvnw clean package
                az logout
            """
        }
    }
    stage('Install Helm Chart'){
        sh """
        helm install kuberhelm helmjenkins
        """
    }
}