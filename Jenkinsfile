node {
    def app
    def source
    stage('Build Docker Image') {
        source = checkout(scm)
        app = docker.build("akshayingle/linux_tweet_app:${env.BUILD_ID}", "--label dockerhp.sample.commit=${source.GIT_COMMIT} .")
    }

    stage('Publish to Docker Hub') {
        docker.withRegistry("https://index.docker.io/v1/", "dockerhub") {
            app.push(env.BUILD_ID)
        }
    }

    stage('Deploy to Production') {
        docker.withServer('tcp://testprod:2376', 'production') {
            sh "docker service update --image akshayingle/linux_tweet_app:dockerHplatest:${env.BUILD_ID} sample"
        }
    }
}
