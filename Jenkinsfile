pipeline {
  agent any
  stages {
    stage('git scm update') {
      steps {
        git url: 'https://github.com/IaC-Source/echo-ip.git', branch: 'main'
      }
    }
    stage('docker build and push') {
      steps {
        groups
        id
        sh "docker ps -a"
        sh "docker build -t harbor-registry.harbor:8080/echo-ip ."
        sh "docker push harbor-registry.harbor:8080/echo-ip"
      }
    }
    stage('deploy kubernetes') {
      steps {
        sh '''
        kubectl create deployment pl-bulk-prod --image=harbor-registry.harbor:8080/echo-ip
        kubectl expose deployment pl-bulk-prod --type=LoadBalancer --port=8080 \
                                               --target-port=80 --name=pl-bulk-prod-svc
        '''
      }
    }
  }
}
