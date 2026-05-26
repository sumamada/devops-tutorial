node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        app = docker.build("madalasuma/test:${BUILD_NUMBER}")
    }

    stage('Test image') {
        bat "docker run --rm madalasuma/test:${BUILD_NUMBER} echo Tests passed"
    }

    stage('Push image') {
        withCredentials([usernamePassword(
            credentialsId: 'docker-hub-credentials',
            usernameVariable: 'DOCKER_USER',
            passwordVariable: 'DOCKER_PASS'
        )]) {

            bat """
            docker login -u %DOCKER_USER% -p %DOCKER_PASS%

            docker tag madalasuma/test:${BUILD_NUMBER} madalasuma/test:latest

            docker push madalasuma/test:${BUILD_NUMBER}
            docker push madalasuma/test:latest
            """
        }
    }
}
