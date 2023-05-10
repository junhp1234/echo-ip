pipeline {
  agent any
  environment {
    HARBOR_CREDENTIAL = credentials('junhp1234')
  }
  stages {
    stage('git scm update') {
      steps {
        git url: 'https://github.com/junhp1234/echo-ip.git', branch: 'any'
      }
    }
    stage('docker login, build and push') {
      steps {
        sh '''
        echo $HARBOR_CREDENTIAL_PSW | docker login harbor.cu.ac.kr -u '$junhp1234' --password-stdin
        docker build -t harbor-registry.harbor.svc.cluster.local:8080/jenkins_test_project/echo-ip .
        docker push harbor-registry.harbor.svc.cluster.local:8080/jenkins_test_project/echo-ip
        '''
      }
    }
    stage('deploy kubernetes') {
      steps {
        sh "kubectl"
        sh "kubectl create deployment pl-bulk-prod --image=harbor-registry.harbor.svc.cluster.local:8080/jenkins_test_project/echo-ip"
        sh "kubectl expose deployment pl-bulk-prod --type=LoadBalancer --port=8080 --target-port=80 --name=pl-bulk-prod-svc"
      }
    }
  }
}
