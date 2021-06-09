pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }
    
    environment{
        ArtifactId=readMavenPom().getArtifactId()
        Version=readMavenPom().getVersion()
        Name=readMavenPom().getName()
        GroupId=readMavenPom().getGroupId()
    }

   stages {
        // Specify various stage with in stages

        // stage 1. Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'

            }
        }

        // Stage3 : Publish the source code to Sonarqube static code amnalysis
        stage ('Sonarqube Analysis'){
           steps {
               echo ' Source code published to Sonarqube for SCA......'
               withSonarQubeEnv('sonarqube'){ // You can override the credential to be used
                     sh 'mvn sonar:sonar'
               }

            }

    
        }

//stage4 :nexus publishing
stage('Publish to Nexus'){
            steps
            {
script{
 def NexusRepo = Version.endsWith("SNAPSHOT") ? "sushmadevopslab_SNAPSHOT" : "sushmadevopslab_RELEASE"
                nexusArtifactUploader artifacts: [[artifactId:"${ArtifactId}", 
                classifier: '',
                 file: "target/${ArtifactId}-${Version}.war", 
                 type: 'war']], 
                 credentialsId: '88a74e93-4a0b-4d8d-9755-67e1304b3c8b', 
                 groupId: "${GroupId}", 
                 nexusUrl: '172.20.10.143:8081', 
                 nexusVersion: 'nexus3', 
                 protocol: 'http', 
                 repository: "${NexusRepo}", 
                 version: "${Version}"
            }
            }
        }

        //stage 5 printing environment variables
stage('print environment variables')
{
steps{
    echo "the artifact is '${ArtifactId}'"
    echo "the version is '${Version}'"
    echo "the group is '${GroupId}'"
    echo "the name is '${Name}'"
}
}
//stage 5 deploying to tomcat
 stage ('deploy to tomcat')
 {
steps
{
echo ("deploying to tomcat")
sshPublisher(publishers:
 [sshPublisherDesc(configName: 'Ansible_controller',
  transfers:[
      sshTransfer(
          cleanRemote:false,
          execCommand:  'ansible-playbook /opt/playbooks/deploy.yml -i /opt/playbooks/hosts',
          execTimeout: 12000000
      )
  ],
    usePromotionTimestamp: false, 
    useWorkspaceInPromotion: false, 
    verbose: false)
 ])
     }}
//stage 7 deploying the build artifact to docker container
stage ('deploy to docker')
 {
steps
{
echo ("deploying to docker")
sshPublisher(publishers:
 [sshPublisherDesc(configName: 'Ansible_controller',
  transfers:[
      sshTransfer(
          cleanRemote:false,
          execCommand:  'ansible-playbook /opt/playbooks/dockerdeploy.yml -i /opt/playbooks/hosts',
          execTimeout: 12000000
      )
  ],
    usePromotionTimestamp: false, 
    useWorkspaceInPromotion: false, 
    verbose: false)
 ])
}
}
   }
}
 
    

