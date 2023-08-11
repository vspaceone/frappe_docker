pipeline {    
  agent { 
    label 'linux' 
  }
  options {
    ansiColor('xterm')
  }
  environment {
    TEST= "test"
  }
  stages {
    stage('Build new container') {	
      steps{
        script {
          var apps_json = """
[
  {
    "url": "https://github.com/frappe/payments",
    "branch": "develop"
  },
  {
    "url": "https://github.com/frappe/erpnext",
    "branch": "version-14"
  },
  {
    "url": "https://github.com/frappe/non_profit",
    "branch": "develop"
  }
]
"""

          def apps_json_enc = sh(script: 'echo $outgoing_json | base64', returnStdout: true, returnStatus: true)

          cmd """buildah build \\
--build-arg=FRAPPE_PATH=https://github.com/frappe/frappe \\
--build-arg=FRAPPE_BRANCH=version-14 \\
--build-arg=PYTHON_VERSION=3.10.12 \\
--build-arg=NODE_VERSION=16.20.1 \\
--build-arg=APPS_JSON_BASE64=$apps_json_enc \\
--tag=git.vspace.one/SpaceIT/frappe_docker/erpnext:1 \\
--file=images/custom/Containerfile .
"""
        }
      }
    }  
  }
}

