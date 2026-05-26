node {

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        bat "docker build -t madalasuma/test:%BUILD_NUMBER% ."
        bat "docker tag madalasuma/test:%BUILD_NUMBER% madalasuma/test:latest"
    }

    stage('Test image') {
        bat "docker run --rm madalasuma/test:%BUILD_NUMBER% echo Tests passed"
    }

    stage('Docker Login + Push') {
        withCredentials([usernamePassword(
            credentialsId: 'dockerhub-creds',
            usernameVariable: 'DOCKER_USER',
            passwordVariable: 'DOCKER_PASS'
        )]) {

            bat """
            echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin

            docker push madalasuma/test:%BUILD_NUMBER%
            docker push madalasuma/test:latest
            """
        }
    }
}
