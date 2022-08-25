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
        stage("Create Container"){
            steps{
                sh '''
                name=$(docker ps -a | awk '{print $NF}' | grep bunglonrebah)
                if [ "$name" == "bunglonrebah" ]; then
                    docker rm bunglonrebah
                fi
                docker run --name bunglonrebah -d -p 81:80 bunglonrebah/intern:v1
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
