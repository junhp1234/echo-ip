pipeline {
  agent any
  environment {
    REGISTRY = 'harbor.cu.ac.kr'
    //REGISTRY = '10.103.220.177:31941'
    USER = 'snslabdocker'
    HARBOR_CREDENTIAL = credentials('junhp1234')
    DOCKER_CREDENTIAL = credentials('67786115-c34d-4035-8ca2-7ac2c882b757')
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
        echo $DOCKER_CREDENTIAL_PSW | docker login -u '$67786115-c34d-4035-8ca2-7ac2c882b757' --password-stdin
        docker build -t echo-ip .
        docker push $USER/echo-ip
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
