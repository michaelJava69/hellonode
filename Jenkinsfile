node {
    def app
    
    environment {
       USER = "michael"
    }
    
    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("ugbechie/hellonode")
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
       /* withDockerRegistry([credentialsId: 'docker-hub-credentials', url: "https://registry.hub.docker.com",
         usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']) {  
          docker.withRegistry('', 'docker-hub-credentials') {
		  sh "docker login  "    }
         sh 'docker login -u "$USERNAME" -p "$PASSWORD"' */    
         
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {  
            app.push("${env.BUILD_NUMBER}")   
            /* sh 'docker login -u "ugbechie" -p " " '  */
            
           /* app.push("1")  */
            app.push("latest")
        }
    }
    /*	
    stage('Remove Unused docker image') {
      steps{
         sh "docker rmi $imagename:$BUILD_NUMBER"
         sh "docker rmi $imagename:latest"
       }
    }
    */
}
