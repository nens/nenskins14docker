# nenskins14docker: N&S jenkins base 14.04 image

We're available on the docker hub:
https://hub.docker.com/r/reinout/nenskins14docker/, we're build automatically
after each github push.

Github repo: https://github.com/nens/nenskins14docker

In our N&S, use it in a ``Jenkinsfile`` more or less like this:

    node("nenskins14docker") {
       stage "Checkout"
       checkout scm

       stage "Build"
       sh "python bootstrap.py"
       sh "bin/buildout"

       stage "Test"
       sh "bin/test | true"
       step $class: 'JUnitResultArchiver', testResults: 'nosetests.xml'
       publishHTML target: [reportDir: 'htmlcov', reportFiles: 'index.html', reportName: 'Coverage report']
    }
