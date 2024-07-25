pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'python:2-alpine'
                }
            }
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'qnib/pytest'
                }
            }
            steps {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Manual Approval') {
            steps {
                script {
                    input message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed'
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'pyinstaller --onefile sources/add2vals.py'
                echo 'Aplikasi akan berjalan selama 1 menit...'
                sleep time: 1, unit: 'MINUTES'  // Jeda selama 1 menit (60 detik)
                echo 'Menghentikan aplikasi...'
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }
    }
}
