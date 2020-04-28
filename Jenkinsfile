pipeline
{
    agent { label 'Linux_Slave' }
	tools
	{
		maven 'Maven3'
	}
	options
    {
        timeout(time: 1, unit: 'HOURS')
		
        // Discard old builds after 5 days or 5 builds count.
        buildDiscarder(logRotator(daysToKeepStr: '5', numToKeepStr: '5'))
	  
	    //To avoid concurrent builds to avoid multiple checkouts
	    disableConcurrentBuilds()
    }
    stages
    {
	    stage ('checkout')
		{
			steps
			{
				checkout scm
			}
		}
		stage ('Build')
		{
			steps
			{
				echo "Building application"
				sh "mvn clean install"
			}
		}
		stage ('Unit Testing')
		{
			steps
			{
				echo "Executing unit tests"
				sh "mvn test"
			}
		}
		stage ('Sonar Analysis')
		{
			steps
			{
				echo "Executing Sonar analysis"
				withSonarQubeEnv("sonar_linux_slave") 
				{
					sh "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar"
				}
			}
		}
		stage ('Upload to Artifactory')
		{
			steps
			{
				rtMavenDeployer (
                    id: 'deployer',
                    serverId: 'artifactory@1012712648',
                    releaseRepo: 'rohangambhir.devopstaskapp2',
                    snapshotRepo: 'rohangambhir.devopstaskapp2'
                )
                rtMavenRun (
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: 'deployer',
                )
                rtPublishBuildInfo (
                    serverId: 'artifactory@1012712648',
                )
			}
		}   
	    
	}
	post 
	{
        success 
         {
            mail body: "Pipeline job for infrastructure validation completed successfully. \nRefer pipeline console logs: http://jenkins.nagarro.com/job/${env.JOB_NAME}/${env.BUILD_NUMBER}/console", subject:"Pipeline ${env.JOB_NAME} deployment completed successfully", to: "rohan.gambhir@nagarro.com"
		 }
         failure 
         {
            mail body: "Pipeline job for infrastructure validation encountered a failure. \nRefer pipeline console logs: http://jenkins.nagarro.com/job/${env.JOB_NAME}/${env.BUILD_NUMBER}/console", subject:"Pipeline ${env.JOB_NAME} deployment failed.", to: "rohan.gambhir@nagarro.com"
			
         }
    }
}
