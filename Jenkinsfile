node {
    def buildDockerImage = 'python:2-alpine'
    def testDockerImage = 'qnib/pytest'

    stage('Build') {
        docker.image(buildDockerImage).inside {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    
    stage('Test') {
        docker.image(testDockerImage).inside {
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
        }
        // Publish test results
        junit 'test-reports/results.xml'
    }
}
