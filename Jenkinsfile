pipeline {
    agent any
   
    stages {
        /*stage('checkout') {
            steps {
                echo "Cloning repo..."
                git url: "https://github.com/bhavani-mudrakola/SamplePythonFlaskApp.git", branch: 'main'
            }
        }*/
        stage('Build Docker Image') {
            steps {
                echo "Build Docker Image"
                bat "docker build -t kubdemoapp:v1 ."
            }
        }
        stage('Docker Login') {
            steps {
                  bat 'docker login -u bhavani765 -p bhanu@123'
                }
            }
        stage('push Docker Image to Docker Hub') {
            steps {
                echo "push Docker Image to Docker Hub"
                bat "docker tag kubdemoapp:v1 bhavani765/sample:kubeimage1"               
                    
                bat "docker push bhavani765/sample:kubeimage1"
                
            }
        }
        stage('Deploy to Kubernetes') { 
            steps { 
                withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                    bat 'kubectl apply -f deployment.yaml --validate=false'
                    bat 'kubectl apply -f service.yaml'
                }
                    // apply deployment & service 
                    //bat 'kubectl apply -f deployment.yaml --validate=false' 
                    //bat 'kubectl apply -f service.yaml' 
            } 
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
