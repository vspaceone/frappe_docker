pipeline {    
  agent { 
    label 'linux' 
  }
  options {
    ansiColor('xterm')
  }
  environment {
    TARGET_IMAGE_NAME= "git.vspace.one/spaceit/frappe_docker/erpnext:1"
  }
  stages {
    stage('Build new container') {	
      steps{
        script {
          def apps_json = """
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
  },
  {
    "url": "https://github.com/jHetzer/erpnextfints",
    "tag": "0.4.1"
  }
]
"""

          sh "echo '$apps_json'"

          def apps_json_enc = sh(script: "echo '$apps_json' | base64", returnStdout: true, returnStatus: false)
          
          echo apps_json_enc

          sh """buildah build \\
--build-arg=FRAPPE_PATH=https://github.com/frappe/frappe \\
--build-arg=FRAPPE_BRANCH=version-14 \\
--build-arg=PYTHON_VERSION=3.10.12 \\
--build-arg=NODE_VERSION=16.20.1 \\
--build-arg=APPS_JSON_BASE64="$apps_json_enc" \\
--tag=$TARGET_IMAGE_NAME \\
--file=images/custom/Containerfile .
"""

          sh "buildah push $TARGET_IMAGE_NAME"
        }
      }
    }  
  }
}

