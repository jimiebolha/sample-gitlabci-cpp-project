podTemplate(label: 'mypod', containers: [
    containerTemplate(name: 'docker', image: 'docker', ttyEnabled: true, command: 'cat'),
    containerTemplate(name: 'kubectl', image: 'jimiealmeida/kubectl:v1.0', command: 'cat', ttyEnabled: true),	
  ],
  volumes: [
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
  ]) {
    node('mypod') {

        stage('Build Busybox') {
            container('docker') {
                    sh """
                    docker build \
                        -t jimiealmeida/busybox:$BUILD_NUMBER \
                        --network=host \
                        -f Dockerfile .
                        """
                }
            }
        }
       stage(Push DocherHub Imagei Busybox) {
           container('docker'){
	      withCredentials([[$class: 'UsernamePasswordMultiBinding',
                        credentialsId: 'jimiealmeida',
                        usernameVariable: 'DOCKER_HUB_USER',
                        passwordVariable: 'DOCKER_HUB_PASSWORD']]) {
                    sh "docker login -u ${env.DOCKER_HUB_USER} -p ${env.DOCKER_HUB_PASSWORD} "
                    sh "docker push ${env.DOCKER_HUB_USER}/busybox:${env.BUILD_NUMBER} "
                }
            }
         }

      }
    
