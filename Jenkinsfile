node {
    
    stage('check out branch') {
        git branch: 'main', url: 'https://github.com/jitinkrishna/capstone-assess.git'
    }
    
    stage('maven build') {
        sh 'mvn clean package'
    }
    
    stage('containerize') {
      //  sh 'docker build -t jitinkrishna3/insure-me:1.0 .'
    }
    
    stage ('Release') {
        withCredentials([string(credentialsId: 'dockerHub', variable: 'dockerhub')]) {
      //  sh "docker login -u jitinkrishna3 -p ${dockerHub}"
      //  sh 'docker push jitinkrishna3/insure-me:1.0'
        }
    }
    
    stage ('Deploy to test server') {
        ansiblePlaybook become: true, credentialsId: 'ansible-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-test-server.yml', vaultTmpPath: ''
    }
    
    stage('checkout test source code') {
       git branch: 'main', url: 'https://github.com/jitinkrishna/sele-test.git'
    }
    
    stage ('build test scripts') {
        sh 'mvn clean package assembly:single'
    }
    
    stage('execute test script'){
        sh 'java -jar target/my-app-test-0.0.1-SNAPSHOT-jar-with-dependencies.jar'
    }
    
     stage('check out branch for prod deploy') {
        git branch: 'main', url: 'https://github.com/jitinkrishna/capstone-assess.git'
    }
    
    stage ('Deploy to PROD server') {
        ansiblePlaybook become: true, credentialsId: 'ansible-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-prod-server.yml', vaultTmpPath: ''
    }
    
}
