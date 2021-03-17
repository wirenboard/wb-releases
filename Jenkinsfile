pipeline {
  agent {
    label "aptly"
  }
  environment {
    APTLY_CONFIG=credentials("release-aptly-config")
    GPG_KEYRING=credentials("repo-secret-keyring")
    PUBLISH_ENDPOINT="s3:deb.wirenboard.com:"
  }
  stages {
    stage("Check packages") {
      steps {
        sh('wbci-repo -c $APTLY_CONFIG check releases.yaml')
      }
    }
    stage("Deploy release") {
      when {
        branch "master"
      }
  
      steps {
        sh('''wbci-repo -c $APTLY_CONFIG \
           -s "{\'SecretKeyring\': \'$GPG_KEYRING\'}" \
           deploy -e $PUBLISH_ENDPOINT releases.yaml''')
      }
    }
  }
}
