pipeline {
    agent any

    environment {
        GCP_PROJECT_ID = 'ascension-poc-prj'
        GKE_CLUSTER    = 'gke-autopilot-private'
        GKE_REGION     = 'us-central1'
        GOOGLE_APPLICATION_CREDENTIALS = "/tmp/gcp-key.json"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Sanghamitra2311/my-Kubernetes-Deployment.git'
            }
        }

        stage('Authenticate with GCP') {
    steps {
        withCredentials([file(credentialsId: 'gcp-service-account-key', variable: 'GCP_KEY')]) {
            sh '''
                echo "Authenticating with GCP..."
                cp $GCP_KEY /tmp/gcp-key.json
                gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS
                gcloud config set project $GCP_PROJECT_ID
            '''
        }
    }
}

        stage('Get GKE Credentials') {
            steps {
                sh '''
                    echo "Fetching GKE cluster credentials..."
                    gcloud container clusters get-credentials $GKE_CLUSTER \
                        --region $GKE_REGION \
                        --project $GCP_PROJECT_ID
                '''
            }
        }

        stage('Deploy to GKE') {
            steps {
                sh '''
                    echo "Applying Kubernetes manifests..."
                    kubectl apply -f configmap.yaml
                    kubectl apply -f deployment.yaml
                    kubectl apply -f service.yaml
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'kubectl get pods'
                sh 'kubectl get services'
            }
        }
    }
}
