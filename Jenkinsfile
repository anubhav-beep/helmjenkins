node('master'){
    
    def servicePrincipalId = 'azureserviceprincipal'
    def resourceGroup = 'jenkinsHelm'
    def aks = 'cluster004'
    
    def acrname = 'jenkinshelmacr'
    
    stage('SCM') {
        checkout scm
    }

    stage('Build') {
        withCredentials([azureServicePrincipal(servicePrincipalId)]) {
            sh """
                az login --service-principal -u "\$AZURE_CLIENT_ID" -p "\$AZURE_CLIENT_SECRET" -t "\$AZURE_TENANT_ID"
                az account set --subscription "\$AZURE_SUBSCRIPTION_ID"
            """
        }
    }
    
    stage('Install Helm Chart'){
        sh """
        az aks get-credentials --resource-group jenkinsHelm --name $aks --admin
        az aks update -n $aks -g jenkinsHelm --attach-acr $acrname
        helm install kuberhelm-${env.BUILD_NUMBER} helmjenkins
        """
    }
}
