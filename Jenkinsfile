pipeline {
    agent { 
        label 'mohan' 
    } 
    parameters {
        string(
            name: 'IMAGE_TAG', 
            defaultValue: 'latest', 
            description: 'Docker image tag to deploy'
        )
    }
    environment {
        HELM_RELEASE = "petclinic"
        HELM_CHART = "./"
        KUBE_CLUSTER = "devil-eks-cluster"
        KUBE_REGION = "us-west-2"
        NAMESPACE = "default"
    }
    stages {
        stage('Configure EKS & Helm Upgrade') {
            steps {
                withCredentials([[$class: 'UsernamePasswordMultiBinding',
                    credentialsId: 'aws_credentials',
                    usernameVariable: 'AWS_ACCESS_KEY_ID',
                    passwordVariable: 'AWS_SECRET_ACCESS_KEY']]) {

                    echo "Configuring EKS cluster and running Helm upgrade with image tag: ${params.IMAGE_TAG}"
                    sh """
                        ls -ltr
                        pwd
                        # Set default AWS region
                        export AWS_DEFAULT_REGION=${KUBE_REGION}

                        # Update kubeconfig for EKS cluster
                        aws eks update-kubeconfig --name ${KUBE_CLUSTER}

                        # Test kubectl connection
                        kubectl get nodes

                        # Run Helm upgrade with image tag parameter
                        # helm repo update
                        helm upgrade --install ${HELM_RELEASE} ${HELM_CHART} \
                            --namespace ${NAMESPACE} \
                            -f conf_values.yaml \
                            --set spring.tag=${params.IMAGE_TAG} \
                            --wait
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed. Check logs for details."
        }
    }
}
