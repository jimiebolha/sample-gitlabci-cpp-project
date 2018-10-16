podTemplate(label: 'mypod', containers: [
    containerTemplate(name: 'kubectl', image: 'jimiealmeida/kubectl:v1.0', command: 'cat', ttyEnabled: true),	
  ],
  volumes: [
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
  ]) {
    node('mypod') {


        stage('Listando Pods') {
           container('kubectl') {
                withCredentials([kubeconfigContent(credentialsId: 'KUBECONFIG_CONTENT', variable: 'KUBECONFIG_CONTENT')]) {
                    sh '''echo "$KUBECONFIG_CONTENT" > ~/config && kubectl --kubeconfig=~/config get pods 
                    '''
              }  
          }
        }
      }
    }
