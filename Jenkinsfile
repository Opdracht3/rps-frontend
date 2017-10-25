node {
    def app
  
    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */
        checkout scm
        $ curl -i localhost:8080
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        // app = docker.build("opdracht3/frontend")
    }
  
    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */
      
      sh 'docker rm demo || true'
      sh 'docker run -t --rm --name demo opdracht3/frontend &'
      sh 'sleep 5s'
      sh 'docker exec -t demo bash -c \'ls -l\''
      sh 'docker stop demo' 
      
    }
    
    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
      withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials',
                     usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {      
        sh "docker login -u=${env.USERNAME} -p=$PASSWORD"
        sh "docker tag opdracht3/frontend tbrewster/frontend:${env.BUILD_NUMBER}"
        //sh "docker push tbrewster/frontend:${env.BUILD_NUMBER}"
      }
    }
}
