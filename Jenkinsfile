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
        docker.environment().inside{
            VOLUME = '$(pwd)/sources:/src'
            IMAGE = 'cdrx/pyinstaller-linux:python2'
        }
        docker.step('path: env.BUILD_ID').inside{
           unstash(name: 'compiled-results') 
           sh "docker run --rm -v ${VOLUME} ${IMAGE} 'pyinstaller -F add2vals.py'" 
        }
    }
}

