node {
    def app

    stage('Clone repository') {
        echo "Cloning repository..."
        checkout scm
    }

    stage('Build image') {
        echo "Building Docker image..."
        app = docker.build("mothukurirk/test")
    }

    stage('Test image') {
        echo "Testing Docker image..."
        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        echo "Pushing Docker image to DockerHub..."
        docker.withRegistry('https://registry.hub.docker.com', 'DockerHub') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }

    stage('Trigger ManifestUpdate') {
        echo "Triggering updatemanifest job..."
        build job: 'updatemanifestjob',
              parameters: [
                  string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)
              ]
    }
}

