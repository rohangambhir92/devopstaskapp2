node('test') 
{
   stage("CheckoutSCM")
   {
       echo "Checking out my code here"
       checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'bbb48b43-0e32-4a0d-b833-3d611916d027', url: 'https://github.com/shreys-s/CounterWebApp.git']]])
   }
   stage("Code Build")
   {
       echo "Starting Maven Code Build here"
       sh 'mvn clean install'
   }
   stage("Unit Testing")
   {
       echo "Starting Unit Test here"
       //junit '*/target/surefire-reports/*.xml'
   }
   stage("Sonarqube Analysis")
   {
       echo "Starting Sonarqube analysis"
       //sh 'mvn sonar:sonar -Dsonar.login=admin -Dsonar.password=admin'
   }
   stage("Artifact Upload")
   {
       echo "Starting Artifact upload"
       //sh 'mvn deploy'
   }
   stage('Prepare Docker Deployment')
    {
        parallel(
            BuildImage:
            {
                //sh 'docker build -t docker.io/shreys/gameoflife:${BUILD_NUMBER} .'
            },
            ContainerCheck:
            {
                //sh 'docker stop game_app || true && docker rm game_app || true'
            }
        )
    }
    stage("Push Image")
    {
       echo "Pushing image to docker Hub"
       //sh 'docker push docker.io/shreys/gameoflife:${BUILD_NUMBER}'
    }
    stage("Docker Deployment")
        {
                //sh 'docker run --name game_app -d -p 12001:8080 docker.io/shreys/gameoflife:${BUILD_NUMBER}'
                //sh 'echo "Please verify running application here: http://192.168.56.105:12001/gameoflife"'
        }
}
