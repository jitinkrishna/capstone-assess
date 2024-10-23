node {
    
    stage('checkout git') {
     git 'https://github.com/jitinkrishna/capstone-assess.git'
    }
    
    stage('maven build') {
      sh 'mvn clean package'
    }
    
    stage('containerize') {
      sh 'docker build -t jitinkrishna3/insure-me:1.0 .'
    }
    
    stage('release') {
       withCredentials([string(credentialsId: 'dockerHubpwd', variable: 'dockerHubpwd')]) {
       sh "docker login -u jitinkrishna3 -p ${dockerHubpwd}"
       sh "docker push jitinkrishna3/insure-me:1.0"
        }
    }
    
    stage('deploy to test') {
      ansiblePlaybook become: true, credentialsId: 'ansible-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-test-server.yml', vaultTmpPath: ''
    }
    
    stage('Regression testing') {
    git 'https://github.com/shubhamkushwah123/my-selenium-test-app.git'
    }
    
    stage('build script'){
        sh 'mvn clean package assembly:single'
    }
    
    stage('execute selenium Test script'){
        sh 'java -jar //var/lib/jenkins/my-app-test-0.0.1-SNAPSHOT-jar-with-dependencies.jar'
    }
    
     stage('checkout git') {
         git 'https://github.com/jitinkrishna/capstone-assess.git'
    }
    
    stage('deploy to prod') {
       ansiblePlaybook become: true, credentialsId: 'ansible-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-prod-server.yml', vaultTmpPath: ''
    }
    
}
