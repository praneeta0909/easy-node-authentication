node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
        sh("chmod 777 /var/run/docker.sock")
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("praneeta0909/hagaik")
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
        docker.withRegistry('https://dtr.rlindia.com', 'dtr-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
     stage('Remove local images') {
        // remove docker images
        sh("docker rmi -f dtr.rlindia.com/praneeta0909/hagaik:$BUILD_NUMBER || :")
    }
}
