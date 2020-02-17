pipeline
{
    agent any
	tools
	{
		maven 'Maven'
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
				withSonarQubeEnv("Test_Sonar") 
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
                    serverId: '123456789@artifactory',
                    releaseRepo: 'rohangambhir.devopstaskapp2',
                    snapshotRepo: 'rohangambhir.devopstaskapp2'
                )
                rtMavenRun (
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: 'deployer',
                )
                rtPublishBuildInfo (
                    serverId: '123456789@artifactory',
                )
			}
		}   
	    
	}
	post 
	{
        always 
		{
			echo "*********** Executing post tasks like Email notifications *****************"
        }
    }
}
