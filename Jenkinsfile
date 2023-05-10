pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          - name: docker
            image: docker:19.03.1
            command:
            - cat
            tty: true
            securityContext:
              privileged: true
            volumeMounts:
             - name: docker-sock
               mountPath: /var/run/docker.sock
          - name: docker-daemon
            image: docker:19.03.1-dind
            env:
            - name: DOCKER_TLS_CERTDIR
              value: ""
  securityContext:
    privileged: true
          volumes:
          - name: docker-sock
            hostPath:
              path: /var/run/docker.sock
        '''
    }
  }
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
