pipeline {
  agent {
    kubernetes {
      yaml """\
        apiVersion: v1
        kind: Pod
        metadata:
          labels:
            "app": "jjb"
            namespace: jenkins
        spec:
          containers:
          - name: jjb
            image: python:3.6
            command: ["/bin/sh", "-ec", "while :; do echo '.'; sleep 5 ; done"]
        """.stripIndent()
    }
  }
  stages {
    stage('install JJB') {
      steps {
        container('jjb') {
          sh 'pip install --timeout=300 jenkins-job-builder'
          sh 'pip install --timeout=300 --src=/tmp/pip -e git+https://github.com/jovandeginste/jenkins-jobs-mattermost.git#egg=jenkins-jobs-mattermost'
          sh 'jenkins-jobs --conf ./jenkins_jobs.ini update -r --delete-old templates:std_pipeline_jobs'
        }
      }
    }
  }
}
