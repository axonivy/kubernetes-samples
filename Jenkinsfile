pipeline {
  agent {
    label 'docker-compose'
  }

  options {
    disableConcurrentBuilds()
    buildDiscarder(logRotator(numToKeepStr: '60', artifactNumToKeepStr: '30'))
  }

  triggers {
    cron 'H H/4 * * *'
  }

  stages {
    stage('pull-image') {
      steps {
        script {
          pullEngineImage()
        }
      }
    }
    
    stage('test-kubernetes') {
      steps {
        script {
          withCredentials([file(credentialsId: 'test-kube01', variable: 'config')]) {
            docker.image('alpine/k8s:1.21.12').inside('-v $config:/.kube/config') {
              def kubernetes = load 'kubernetes.groovy'
              examples().each { entry ->
                def example = entry.key
                def assertion = entry.value
                kubernetes.newKubernetes(example).runTest(assertion)
              }
            }
          }
        }
      }
    }

    stage('archive') {
      steps {        
        archiveArtifacts allowEmptyArchive: true, artifacts: 'warn*.log'
      }
    }
  }
}

def pullEngineImage() {
  sh 'docker pull axonivy/axonivy-engine:dev'
}

def examples() {
  return [
    'ivy': { log -> assertIvyIsRunningInDemoMode() },
  ]
}

def assertIvyIsRunningInDemoMode() {
  if (!isIvyRunningInDemoMode()) {
    throw new Exception("ivy is not running in demo mode")
  }
  if (isIvyRunningInMaintenanceMode()) {
    throw new Exception("ivy is running in maintenance mode");
  }
}

def isIvyRunningInDemoMode() {
  isIvyRunningInDemoModeOnPort(8080)
}

def isIvyRunningInDemoModeOnPort(port) {
  def response = sh (script: "wget -qO- http://localhost:$port/info/index.jsp", returnStdout: true)
  return response.contains('Demo Mode')
}

def isIvyRunningInMaintenanceMode() {
  isIvyRunningInMaintenanceModeOnPort(8080)
}

def isIvyRunningInMaintenanceModeOnPort(port) {
  def response = sh (script: "wget -qO- http://localhost:$port/info/index.jsp", returnStdout: true)
  return response.contains('Maintenance Mode')
}

