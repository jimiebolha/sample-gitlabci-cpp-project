podTemplate(label: 'mypod', containers: [
    containerTemplate(name: 'docker', image: 'docker', ttyEnabled: true, command: 'cat'),
    containerTemplate(name: 'kubectl', image: 'jimiealmeida/kubectl:v1.0', command: 'cat', ttyEnabled: true),	
    containerTemplate(name: 'helm', image: 'lachlanevenson/k8s-helm:latest', command: 'cat', ttyEnabled: true)
  ],
  volumes: [
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
  ]) {
    node('mypod') {

        stage('do some Docker work') {
            container('docker') {

                withCredentials([[$class: 'UsernamePasswordMultiBinding', 
                        credentialsId: 'jimiealmeida',
                        usernameVariable: 'DOCKER_HUB_USER', 
                        passwordVariable: 'DOCKER_HUB_PASSWORD']]) {
                    
                    sh """
                        docker pull ubuntu
                        docker tag ubuntu ${env.DOCKER_HUB_USER}/ubuntu:${env.BUILD_NUMBER}
                        """
                    sh "docker login -u ${env.DOCKER_HUB_USER} -p ${env.DOCKER_HUB_PASSWORD} "
                    sh "docker push ${env.DOCKER_HUB_USER}/ubuntu:${env.BUILD_NUMBER} "
                }
            }
        }

        stage('Listando Pods') {
           container('kubectl') {
                withCredentials([kubeconfigContent(credentialsId: 'KUBECONFIG_CONTENT', variable: 'KUBECONFIG_CONTENT')]) {
                    sh '''echo "$KUBECONFIG_CONTENT" > ~/config && kubectl config set-cluster cluster --server="https://192.168.99.100:8443" --insecure-skip-tls-verify=true && kubectl config set-credentials jenkins --token="eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImplbmtpbnMtdG9rZW4tOXNzbmMiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiamVua2lucyIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6ImUyYTBjYzk0LWNiNWMtMTFlOC1iZGY3LTA4MDAyNzhhZmQ0YyIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDpkZWZhdWx0OmplbmtpbnMifQ.wjJZTeL17_Z7h8Sea3Tq73JKtJEyIpeKJaH9hxZ5cMJ7OQVECaXWpbByFUxc4d9q2yc2CDt3mtElbbYbTdB7UiYq201Dv2xSCBQfjN-LTwsx5c6r_hYslYCZ0D0pmGG9WBYFwlc6q8G03jssDitflbxoPxUs6vc132HUFSL1ONTNqXvYuqSTrnXI50unrfatvw4bjzXOnC6FlM5SpjzmTHYKXJ7f8-_YKZ4HdnG2EcVIXKD0wX57FQKfgJOtMRg7cHB__lzQXSK1VgLgRz61lM2gavqyBurBLJuzPvokYiCPq_JiQUTEiBBLxcFd14XsAAGOmU3U02trZm0O1XHSCA" && kubectl config set-context svcs-acct-context --cluster=cluster --user=jenkins --namespace=default && kubectl config use-context cluster && kubectl get pods
                    '''
              }  
          }
        }
      }
    }
