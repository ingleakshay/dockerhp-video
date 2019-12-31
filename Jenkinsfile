node {
    def app
    stage('Build Docker Image') {
        checkout scm
        app = docker.build('akshayingle/linux_tweet_app:dockerHplatest')
    }
    
    stage('Publish to Docker Hub') {
        docker.withRegistry("https://index.docker.io/v1/", "dockerhub") {
            app.push('dockerHplatest')
        }
    }

    stage('Deploy to Production') {
        docker.withServer('tcp://34.70.253.16:2376', 'production') {
           /* sh 'docker run -d akshayingle/linux_tweet_app:dockerHplatest'&*/
             docker.image('akshayingle/linux_tweet_app:dockerHplatest').withRun('-p 3306:3306') {
            /* do things */
                 sh 'make check'
        }
                
                
        }
    }
}
