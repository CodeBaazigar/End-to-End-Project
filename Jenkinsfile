pipeline {
    agent any
    stages{
        stage('Build Maven'){
            steps{
                git url:'https://github.com/CodeBaazigar/End-to-End-Project.git', branch: "master"
               sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t codebaazigar/april302025project:v1 .'
                }
            }
        }
          stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push codebaazigar/april302025project:v1'
                }
            }
        }
        
        
        stage('Deploy to k8s'){
            steps{
                script{
                     kubernetesDeploy (configs: '', kubeconfigId: 'k8sconfigpwd')
                   
                }
            }
        }
    }
}
