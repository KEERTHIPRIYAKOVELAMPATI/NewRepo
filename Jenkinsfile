@Library('piper-lib-os') _

node() {
  stage('init') {
    deleteDir()
	checkout scm
	def folder = "trial-flow4";
    def filePath = folder + ".zip";
    zip dir: folder, glob: '', zipFile: filePath;
  }
stage("build") {
  try {
    sh 'sh script.sh'  
  }
  catch (err) {
    currentBuild.result = 'FAILURE'
    emailExtraMsg = "Build Failure:"+ err.getMessage()
    throw err
  }
}

  stage('deployIntegrationArtifact and Get MPL Status') {
  	 setupCommonPipelineEnvironment script: this
	   integrationArtifactUpload script: this
     integrationArtifactDeploy script: this
	   integrationArtifactGetMplStatus script: this
	   print "MPL Status:"
	   print  commonPipelineEnvironment.getValue("integrationFlowMplStatus")
  }
}