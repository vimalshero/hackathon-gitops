pipeline {
    agent any
    
    environment {
        APP_NAME = "hackathon-starter"
    }

    stages {
        stage('Cleanup Workspace') {
            stes {
              cleanWs()
            }
        }
    }
        stage('scm') {
            steps{
                git branch: 'main', credentialsId: 'git', url: 'https://github.com/vimalshero/hackathon-gitops.git'
            }
        }
        stage('deployment tag'){
            steps {
                 sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }
        }
        stage('deployment'){
            steps{
                sh """
                    git config --global user.name "vimalshero"
                    git config --global user.email "rajkumarvimal77@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/vimalshero/hackathon-gitops.git"
                }
            }
        }
    }
}
