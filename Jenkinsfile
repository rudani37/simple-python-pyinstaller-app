node {
    stage('Build') {
        docker.image('python:2-alpine').inside {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }        
    }

    stage('Test') {
        docker.image('qnib/pytest').inside {
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
        }
    }

    stage('Deliver') {
        docker.VOLUME('$(pwd)/sources:/src').image('cdrx/pyinstaller-linux:python2').dir(path: env.BUILD_ID) {
           sh "docker run --rm -v ${VOLUME} ${IMAGE} 'pyinstaller -F add2vals.py'" 
        }
    }
}
