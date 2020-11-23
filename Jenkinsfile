pipeline {
    agent any
    
    stages {
        stage('Fetch dependencies') {
        /* This stage pulls the latest image from
           Dockerhub */
            steps {
                sh 'sudo docker pull karthequian/helloworld:latest'
          }
        }
        stage('Build docker image') {
        /* This stage builds the actual image; synonymous to
           docker build on the command line */
            steps {
            sh "sudo docker build . -t customapp:1"
            }    
        }
        stage('Test image') {
         /* This stage runs unit tests on the image; we are
            just running dummy tests here */
            steps {
                sh 'echo "Tests successful"'
          }
        }
        stage('Push image to OCIR') {
         /* Final stage of build; Push the 
            docker image to our OCI private Registry*/
        steps {
            sh "sudo docker login -u 'ax96vm4vpc0w/achuabebe' -p 'y4II8;-A5TI_eZRtNIWQ' jed.ocir.io"
            sh "sudo docker tag customapp:1 jed.ocir.io/ax96vm4vpc0w/customapp:custom"
            sh 'sudo docker push jed.ocir.io/ax96vm4vpc0w/customapp:custom'
            
           }
         } 
         stage('Deploy to OKE') {
         /* Deploy the image to OKE*/

        steps {
            /*testing node'"*/
            kubectl create secret docker-registry secret --docker-server=jed.ocir.io --docker-username='ax96vm4vpc0w/achuabebe' --docker-password='y4II8;-A5TI_eZRtNIWQ' --docker-email='achu@lendoapp.com'
            sh 'sudo docker login -u 'ax96vm4vpc0w' -p "y4II8;-A5TI_eZRtNIWQ" jed.ocir.io'
            sh 'kubectl apply -f /var/lib/jenkins/hello.yml'
           }
         }     
    }
}
