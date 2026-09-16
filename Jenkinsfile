pipeline {
    agent any
    environment {
    AWS_REGION = 'ap-south-1'
    AWS_REGISTRY = '613719615634.dkr.ecr.ap-south-1.amazonaws.com'
    AWS_REPOSTRY = 'app'
    IMAGE_NUMBER = "${BUILD_NUMBER}"
    IMAGE = "${AWS_REGISTRY}/${AWS_REPOSTRY}:${IMAGE_NUMBER}"    
    }
    stages {
        stage('checkout'){
            steps {
            
                checkout scm
            }
        
        
        }
        stage('test'){
            steps {
            
                sh 'pytest -q'
            
            }
        
        }
        stage('image build'){
            steps {
            
                sh 'docker build -t ${IMAGE} .'

            
            }
        
        }
        stage('scan'){
            steps {
                sh 'trivy image --exit-code 1 --severity HIGH,CRITICAL ${IMAGE}'
                
            }
        
        }
        stage('login ecr'){
            steps {
                sh '''
                    aws ecr get-login-password \
                    --region ${AWS_REGION} |
                    docker login --username AWS --password-stdin ${AWS_REGISTRY}

                '''
            
            
            }

        
        }
        stage('push'){
            steps {
            
                sh 'docker push ${IMAGE}'
        
            }
        }
        stage('deploy on stage '){
        
            steps {
                sh'''
                    export IMAGE=${IMAGE}
                    docker compose pull
                    docker compose up -d
                '''
            }
        
        }
        stage('heath check'){
            steps {
                sh '''
                    sleep 10
                    docker compose ps
                    curl -f http://localhost:5000
                    
                '''
            
            }
        
        
        }
    
    
    
    
    
    
    }

}
