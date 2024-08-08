pipeline {
    agent any

    parameters {
        string(name: 'SERVICE_NAME', defaultValue: '', description: '')
        string(name: 'IMAGE_FULL_NAME_PARAM', defaultValue: '', description: '')
    }

    stages {
        stage('Git setup') {
            steps {
                sh 'git checkout -b main || git checkout main'
            }
        }
        stage('Update YAML manifests') {
            steps {
                sh '''
                cd k8s/${SERVICE_NAME}
                if [[ "$OSTYPE" == "darwin"* ]]; then
                    sed -i '' "s|image: .*|image: ${IMAGE_FULL_NAME_PARAM}|" deployment.yaml
                else
                    sed -i "s|image: .*|image: ${IMAGE_FULL_NAME_PARAM}|" deployment.yaml
                fi
                git add deployment.yaml
                git commit -m "Jenkins deploy $SERVICE_NAME $IMAGE_FULL_NAME_PARAM"
                '''
            }
        }
        stage('Git push') {
            steps {
               withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GITHUB_USERNAME', passwordVariable: 'GITHUB_TOKEN')]) {
                 sh '''
                 git push https://$GITHUB_TOKEN@github.com/talorlik/NetflixInfra.git main
                 '''
               }
            }
        }
    }
    post {
        cleanup {
            cleanWs()
        }
    }
}