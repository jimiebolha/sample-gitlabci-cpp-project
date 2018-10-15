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
             sh "kubectl --token=ZXlKaGJHY2lPaUpTVXpJMU5pSXNJbXRwWkNJNklpSjkuZXlKcGMzTWlPaUpyZFdKbGNtNWxkR1Z6TDNObGNuWnBZMlZoWTJOdmRXNTBJaXdpYTNWaVpYSnVaWFJsY3k1cGJ5OXpaWEoyYVdObFlXTmpiM1Z1ZEM5dVlXMWxjM0JoWTJVaU9pSmtaV1poZFd4MElpd2lhM1ZpWlhKdVpYUmxjeTVwYnk5elpYSjJhV05sWVdOamIzVnVkQzl6WldOeVpYUXVibUZ0WlNJNkltcGxibXRwYm5NdGRHOXJaVzR0T1hOemJtTWlMQ0pyZFdKbGNtNWxkR1Z6TG1sdkwzTmxjblpwWTJWaFkyTnZkVzUwTDNObGNuWnBZMlV0WVdOamIzVnVkQzV1WVcxbElqb2lhbVZ1YTJsdWN5SXNJbXQxWW1WeWJtVjBaWE11YVc4dmMyVnlkbWxqWldGalkyOTFiblF2YzJWeWRtbGpaUzFoWTJOdmRXNTBMblZwWkNJNkltVXlZVEJqWXprMExXTmlOV010TVRGbE9DMWlaR1kzTFRBNE1EQXlOemhoWm1RMFl5SXNJbk4xWWlJNkluTjVjM1JsYlRwelpYSjJhV05sWVdOamIzVnVkRHBrWldaaGRXeDBPbXBsYm10cGJuTWlmUS53akpaVGVMMTdfWjdoOFNlYTNUcTczSkt0SkV5SXBlS0phSDloeFo1Y01KN09RVkVDYVhXcGJCeUZVeGM0ZDlxMnljMkNEdDNtdEVsYmJZYlRkQjdVaVlxMjAxRHYyeFNDQlFmak4tTFR3c3g1YzZyX2hZc2xZQ1owRDBwbUdHOVdCWUZ3bGM2cThHMDNqc3NEaXRmbGJ4b1B4VXM2dmMxMzJIVUZTTDFPTlROcVh2WXVxU1RyblhJNTB1bnJmYXR2dzRianpYT25DNkZsTTVTcGp6bVRIWUtYSjdmOC1fWUtaNEhkbkcyRWNWSVhLRDB3WDU3RlFLZmdKT3RNUmc3Y0hCX19selFYU0sxVmdMZ1J6NjFsTTJnYXZxeUJ1ckJMSnV6UHZva1lpQ1BxX0ppUVVURWlCQkx4Y0ZkMTRYc0FBR09tVTNVMDJ0clptME8xWEhTQ0E= -n default get pods"
          }
        }
      }
    }
