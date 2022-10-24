node {
    withDockerContainer('python:2-alpine') {
        stage('Build') {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    withDockerContainer('qnib/pytest') {
        stage('Test') {
            sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
        }
    }
    stage('Manual Approval') {
        input message: 'Lanjutkan ke tahap Deploy?'
    }
    stage('Deploy') {
        sh 'docker run --rm -v /var/jenkins_home/workspace/submission-cicd-pipeline-rudani/sources:/src cdrx/pyinstaller-linux:python2 \'pyinstaller -F add2vals.py\''
        archiveArtifacts artifacts: 'sources/add2vals.py'
        sh 'docker run --rm -v /var/jenkins_home/workspace/submission-cicd-pipeline-rudani/sources:/src cdrx/pyinstaller-linux:python2 \'rm -rf build dist\''
        sleep 60
    }
}