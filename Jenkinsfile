// For Dicoding Submission
pipeline {
  agent any;
  stages {
    // Building Image
    stage('Build') {
      agent {
          docker {
              image 'python:2.7-alpine'
          }
      }
      steps {
          sh 'python -m py_compile sources/add2vals.py sources/calc.py'
      }
    }

    // Testing App
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

    // Deliver Artifact
    stage('Deliver') {
      agent {
          docker {
              image 'cdrx/pyinstaller-linux:python2'
              args "-u root --privileged"
          }
      }
      steps {
          sh '/root/.pyenv/shims/pyinstaller -F sources/add2vals.py'
      }
      post {
          success {
              archiveArtifacts 'dist/add2vals'
          }
      }
    }
  }
}
