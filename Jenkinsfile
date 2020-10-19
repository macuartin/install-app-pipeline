#!/user/bin/env groovy

def COLOR_MAP = ['SUCCESS': 'good', 'FAILURE': 'danger', 'UNSTABLE': 'danger', 'ABORTED': 'danger']

pipeline {

  agent any

  stages {
    stage('download artifact') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'JFROG_CREDENTIALS', usernameVariable: 'JFROG_USERNAME', passwordVariable: 'JFROG_PASSWORD')]) {
          sh(script: "wget --user ${JFROG_USERNAME} --password ${JFROG_PASSWORD} ${ARTIFACT_URL} -O artifact.zip")
        }
      }
    }

    stage('deploy artifact') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'SSH_CREDENTIALS', usernameVariable: 'SSH_USERNAME', passwordVariable: 'SSH_PASSWORD')]) {
          sh "ansible-playbook -i ./inventories/hosts main.yml --tags '${COMPONENT}' --extra-vars 'HOST=${HOST} REMOTE_PATH=${REMOTE_PATH} SSH_USERNAME=${SSH_USERNAME} SSH_PASSWORD=${SSH_PASSWORD}'"
        }
      }
    } 
  }

  post { 
    always { 
      cleanWs()
    }
  }
}
