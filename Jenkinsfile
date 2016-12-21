node {
  def workspace = pwd()
  sh "echo ${workspace}"
  stage('Build') {
    checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'playbooks/build']], submoduleCfg: [], userRemoteConfigs: [[name: 'cosnics-build', url: 'https://github.com/pjbro/ansible-playbook-cosnics-build.git']]])
    sh "ANSIBLE_FORCE_COLOR=true"
    sh "ansible-playbook playbooks/build/build.yml --extra-vars 'branch=${BRANCH} workspace=${workspace}/cosnics-build-source repo=https://github.com/cosnics/cosnics.git'"
  }

  stage('Test') {
    sh 'echo testing'
  }

  stage('Deploy') {
    checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'playbooks/deploy']], submoduleCfg: [], userRemoteConfigs: [[name: 'cosnics-deploy', url: 'https://github.com/pjbro/ansible-playbook-cosnics-deploy.git']]])
    sh "ANSIBLE_FORCE_COLOR=true"
    sh "ansible-galaxy install -r playbooks/deploy/requirements.yml --roles-path ${workspace}/../ansible-roles --force"
    sh "ansible-playbook playbooks/deploy/deploy.yml -i playbooks/deploy/hosts --extra-vars 'hosts=test project_root=/var/www/html/cosnics-${BRANCH} project_local_path=${workspace}/cosnics-build-source/${BRANCH}/ remote_user=elearning_deployer'"
  }
}
