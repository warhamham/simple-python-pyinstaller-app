node {
    def buildDockerImage = 'python:2-alpine'
    def testDockerImage = 'qnib/pytest'
    
    stage('Build') {
        docker.image(buildDockerImage).inside {
            // Verifikasi isi direktori
            sh 'ls -la sources'
            // Kompilasi file Python
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    
    stage('Test') {
        docker.image(testDockerImage).inside {
            // Verifikasi isi direktori
            sh 'ls -la sources'
            // Jalankan pengujian
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
        }
        // Publikasikan hasil pengujian
        junit 'test-reports/results.xml'
    }
}
