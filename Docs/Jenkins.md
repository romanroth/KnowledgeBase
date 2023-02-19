# Jenkins

- https://plugins.jenkins.io/htmlpublisher/
- Jenkins Script: <jenkinsurl>/script
  - Jenkins CORS für Doxygen: `System.setProperty("hudson.model.DirectoryBrowserSupport.CSP", "default-src * 'unsafe-inline' 'unsafe-eval';")`
- `groovy.lang.MissingPropertyException` means the variable was not declared
- On Errors: look for `at WorkflowScript.run(WorkflowScript:<linenumber>)`

## Build with docker container

- Use scp to copy files to and from the container
- Use certificates
- **TODO: HOW TO SET THIS UP**

**TODO: Pipeline example**

## Build docker container and push to own registry

```groovy
// 'master' is the name of the built in node in jenkins before version 2.40 .
// See: https://www.jenkins.io/doc/book/managing/built-in-node-migration/
// Also see: https://www.docker.com/blog/multi-platform-docker-builds
// Check the tags on the server:
// 		curl -X GET https://<address>:<port>/v2/<container-name>/tags/list

node('master')
{
    // Root directory of the server to check out from.
    def svnRootDirectory = 'http://<svn-repo-ip>/<path-to-repo>'
	// Current HEAD revision of the repository (set after checkout).
	def svnHeadRevision
    
    properties([
    	[
			// Only keep the last 10 builds of this job.
        	$class: 'jenkins.model.BuildDiscarderProperty',
        	strategy: 
        	[
          		$class: 'LogRotator',
          		numToKeepStr: '10'
        	]
			
    	]
    ])
    environment{
    }

    // Checkout Stage.
    stage('Checkout')
	{
        deleteDir()
		
		def svnVars = checkout(
								[
									$class: 'SubversionSCM',
									additionalCredentials: [],
									excludedCommitMessages: '',
									excludedRegions: '',
									excludedRevprop: '',
									excludedUsers: '',
									filterChangelog: false,
									ignoreDirPropChanges: false,
									includedRegions: '',
									locations:
									[[
										cancelProcessOnExternalsFail: true,
										credentialsId: '3a7f7147-152c-4e29-8a13-16c90caadba8',
										depthOption: 'infinity',
										ignoreExternalsOption: false,
										local: '.',
										remote: svnRootDirectory
									]],
									quietOperation: false,
									workspaceUpdater: [$class: 'UpdateUpdater']
								]
							)
		svnHeadRevision = svnVars.SVN_REVISION
    }

    // Build container.
    stage('Build Container')
	{
		def buildCommand = 'docker build -f ./<dockerfile> -t <container-name>:' + svnHeadRevision + " ."
		sh(buildCommand)		
    }

	// Push to registry for use.
	stage('Push Container')
	{
		// Login to the nexus.
		sh('docker login <address>:<port> --username <username> --password <password>')

		// Tag the image.
		def tagCommand = 'docker tag <container-name>:' + svnHeadRevision + ' <address>:<port>/<container-name>:' + svnHeadRevision
		sh(tagCommand)
		tagCommand = 'docker tag <container-name>:' + svnHeadRevision + ' <address>:<port>/<container-name>:latest'
		sh(tagCommand)

		// Push the image tagged with latest and with its revision.
		sh('docker image push <address>:<port>/<container-name>:latest')
		def pushRevCommand = 'docker image push <address>:<port>/<container-name>:' + svnHeadRevision
		sh(pushRevCommand)
	}	
}
```