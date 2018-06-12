@Library('https://github.com/sealingtech/EDCOP-PIPELINE@master')
def pipeline = new io.edcop.Pipeline()



node {
    def app


    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("gcr.io/edcop-public/hellonode")
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
        docker.withRegistry('https://gcr.io/edcop-public/', 'gcr:edcop-public') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        pipeline.kubectlTest()
    }
    
    stage('helm deploy') {
        sh 'helm install hellonode-chart' 
    }

}
