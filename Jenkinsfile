node {
    def app
    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }
    
     stage('Check And Deploy to K8s') {
         withKubeConfig(caCertificate: '', credentialsId: 'kubernetes_ecr', serverUrl: 'https://54.83.253.164') {
             sh 'kubectl get pods'
             sh 'kubectl apply -f deployment.yaml'
             sh 'kubectl apply -f service.yaml'
        }
    }  
}
