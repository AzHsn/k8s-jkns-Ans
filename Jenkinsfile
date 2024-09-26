pipeline {
    agent any

    parameters {
        string(name: 'IMAGE_NAME', defaultValue: 'nginx', description: 'An Image name')
        string(name: 'IMAGE_VERSION', defaultValue: '1.14.2', description: 'An Image version')
    }
    
    environment {
        DockerFile = './Dockerfile-Nginx'
        DeploymentFile = './deployment.yml'
        Inventory = './inventory.ini'
        AnsiblePlaybook = './main.yml'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_VERSION -f $DockerFile .'
            }
        }
        
        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DOCKERHUB', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'docker login -u $USERNAME -p $PASSWORD'
                }
            }
        }
        
        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DOCKERHUB', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh 'docker tag $IMAGE_NAME:$IMAGE_VERSION $USERNAME/$IMAGE_NAME:$IMAGE_VERSION && docker push $USERNAME/$IMAGE_NAME:$IMAGE_VERSION'
                }

            }

        }

        stage('Prepare DeploymentFile') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DOCKERHUB', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh 'cp $DeploymentFile .'
                sh 'sed "s|image: nginx:1.14.2|image: $USERNAME/$IMAGE_NAME:$IMAGE_VERSION|" deployment.yml > /home/abdelazez/out.yml' 
                }
            }
        }

        stage('Deploy on K8S') {
            steps {
                sh 'ansible-playbook -i $Inventory $AnsiblePlaybook'
            }
        }
    }
}
