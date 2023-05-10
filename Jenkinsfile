pipeline {
  agent any
  environment {
    REGISTRY = '10.111.86.14:8080'
    //REGISTRY = '10.103.220.177:31941'
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
        echo $HARBOR_CREDENTIAL_PSW | docker login $REGISTRY -u '$junhp1234' --password-stdin
        docker build -t $REGISTRY/jenkins_test_project/echo-ip .
        docker push $REGISTRY/jenkins_test_project/echo-ip
        '''
      }
    }
    stage('deploy kubernetes') {
      steps {
        sh "kubectl"
        sh "kubectl create deployment pl-bulk-prod --image=$REGISTRY/jenkins_test_project/echo-ip"
        sh "kubectl expose deployment pl-bulk-prod --type=LoadBalancer --port=8080 --target-port=80 --name=pl-bulk-prod-svc"
      }
    }
  }
}
