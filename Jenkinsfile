node {
    def buildDockerImage = 'python:2-alpine'
    def testDockerImage = 'qnib/pytest'

    stage('Build') {
        docker.image(buildDockerImage).inside {
            sh 'ls -la $WORKSPACE/sources'  
            sh 'python -m py_compile $WORKSPACE/sources/add2vals.py $WORKSPACE/sources/calc.py'
        }
    }
    
    stage('Test') {
        docker.image(testDockerImage).inside {
            sh 'ls -la $WORKSPACE/sources'
            sh 'py.test --verbose --junit-xml $WORKSPACE/test-reports/results.xml $WORKSPACE/sources/test_calc.py'
        }
        // Publish test results
        junit 'test-reports/results.xml'
    }
}
