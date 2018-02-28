node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

     stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("iavorskiy/app_new")
    }



    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }

     stage('Deploy image'){

         sh "docker pull iavorskiy/app_new"
         sh "docker-compose up -d"
         sh "docker-compose scale app=3"

    }

    stage('Deploy image'){

         sh "docker ps > test.txt"
         sh "python nginx_upstream.py"
         sh "docker-compose restart web"

    }



}