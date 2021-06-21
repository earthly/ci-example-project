node('earthly && linux && docker') {
    stage('Build with Earthly') {
        sh 'earthly github.com/earthly/ci-example-project:main+docker'
    }
}