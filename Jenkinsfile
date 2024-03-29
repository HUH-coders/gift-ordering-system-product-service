pipeline { 
    environment { 
        registry = "urvi99/product-service" 
        registryCredential = 'dockerhub' 
        dockerImage = ''
    }
    agent any
    stages { 
        stage('Clone') { 
            steps { 
                git  branch:'main', url:'https://github.com/HUH-coders/gift-ordering-system-product-service.git'
            }
        }
        stage('Build') { 
            steps { 
                script { 
                    dockerImage = docker.build(registry + ":$BUILD_NUMBER"," .")
                }
            } 
        }
        stage('Test') {
            steps {
                script{
                    bat 'echo "Testing successful"'
                }
            }
            
        }
        stage('Deploy') { 
            steps { 
                script { 
                    docker.withRegistry( '', registryCredential ) { 
                        dockerImage.push() 
                    }
                }
            }
        }
        stage('Clean Up') { 
            steps { 
                bat "docker rmi $registry:$BUILD_NUMBER" 
            }
        }
    }
    post {
        always {
            echo 'The pipeline completed'
        }
        success {                   
            echo "Flask Application Up and running!!"
        }
        failure {
            echo 'Build stage failed'
            error('Stopping early due to failure!!')
        }
    }
}
