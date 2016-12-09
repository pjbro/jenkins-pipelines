node {
  stage('Build') {
    checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'playbooks/build']], submoduleCfg: [], userRemoteConfigs: [[name: 'cosnics-build', url: 'https://github.com/pjbro/cosnics-build.git']]])
    checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'playbooks/deploy']], submoduleCfg: [], userRemoteConfigs: [[name: 'cosnics-deploy', url: 'https://github.com/pjbro/cosnics-deploy.git']]])
    sh "ANSIBLE_FORCE_COLOR=true"
    sh "ansible-playbook playbooks/build/build.yml --extra-vars 'branch=${BRANCH} workspace=cosnics-${BRANCH} repo=https://github.com/vanpouckesven/cosnics.git'"
  }

  stage('Test') {
    sh 'echo testing'
  }

  stage('Deploy') {
  }
}
