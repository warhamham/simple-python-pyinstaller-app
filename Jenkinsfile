node {
    def buildDockerImage = 'python:2-alpine'
    def testDockerImage = 'qnib/pytest'
    def deliverDockerImage = 'cdrx/pyinstaller-linux:python2'

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

    stage('Manual Approval') {
        input message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed'
    }

    stage('Deploy') {
        docker.image(deliverDockerImage).inside {
            sh 'pyinstaller --onefile sources/add2vals.py'
            echo 'Aplikasi akan berjalan selama 1 menit...'
            sleep 60  // Jeda selama 1 menit (60 detik)
            echo 'Mengakhiri aplikasi...'
            sh 'pkill add2vals' // Sesuaikan dengan perintah untuk menghentikan aplikasi Anda jika berbeda
        }
        // Archive the built artifact
        archiveArtifacts 'dist/add2vals'
    }
}
