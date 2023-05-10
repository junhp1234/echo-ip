podTemplate(label: 'jenkins-slave-pod', 
  containers: [
    containerTemplate(
      name: 'docker',
      image: 'docker',
      command: 'cat',
      ttyEnabled: true
    )
  ],
  volumes: [ 
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'), 
  ]
)
pipeline {
  stages {
    stage('git scm update') {
      steps {
        git url: 'https://github.com/junhp1234/echo-ip.git', branch: 'main'
      }
    }
    stage('docker build and push') {
      steps {
        container('docker') {
          sh "docker build -t harbor-registry.harbor:8080/jenkins_test_project/echo-ip ."
          sh "docker push harbor-registry.harbor:8080/jenkins_test_project/echo-ip"
        }
      }
    }
    stage('deploy kubernetes') {
      steps {
        sh '''
        kubectl create deployment pl-bulk-prod --image=harbor-registry.harbor:8080/jenkins_test_project/echo-ip
        kubectl expose deployment pl-bulk-prod --type=LoadBalancer --port=8080 \
                                               --target-port=80 --name=pl-bulk-prod-svc
        '''
      }
    }
  }
}
