pipeline{
    agent any
    stages{
        stage("SCM Checkout"){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'CleanBeforeCheckout', deleteUntrackedNestedRepositories: true]], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'github-login', url: 'https://github.com/ajengfrr/Dockerfile/']]])
            }
        }
        stage("Create Image"){
            steps{
                sh '''
                docker build -t bunglonrebah/intern:v1 .
                '''
            }
        }
        stage("Push Image"){
            steps{
                sh '''
                set +x
                docker login --username=bunglonrebah --password=$dockerPassword
                set -x
                docker push bunglonrebah/intern:v1
                '''
            }
        }
    }
}
