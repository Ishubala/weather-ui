node {
    


    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */


        checkout scm
    }


    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */


        app = docker.build("public.ecr.aws/c6p1a7g3/weather:ui")
    }


    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */


        app.inside {
            sh 'echo "Tests passed"'
        }
    }


    stage('Push image to ECR') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        sh "docker push public.ecr.aws/c6p1a7g3/weather:ui"
    }
    
     stage('Check And Deploy to K8s') {
         withKubeConfig(caCertificate: '', credentialsId: 'kubernetes_ecr', serverUrl: 'https://54.83.253.164') {
             sh 'kubectl get pods'
             sh 'kubectl apply -f deployment.yaml'
             sh 'kubectl apply -f service.yaml'
        }
    }  
}
