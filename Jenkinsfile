podTemplate(label: 'jenkins-slave-pod', 
  containers: [
    containerTemplate(
      name: 'docker',
      image: 'docker',
      command: 'cat',
      ttyEnabled: true
    ),
  ],
  volumes: [ 
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'), 
  ]
)

{
    node('jenkins-slave-pod') { 
        def registry = "harbor-registry.harbor:8080"

        // https://jenkins.io/doc/pipeline/steps/git/
        stage('Clone repository') {
            container('docker') {
              sh "docker build -t harbor-registry.harbor:8080/jenkins_test_project/echo-ip ."
            }
        }
    }   
}
