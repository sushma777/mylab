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

        // Stage3 : Publish the source code to Sonarqube
       // stage ('Sonarqube Analysis'){
         //   steps {
           //     echo ' Source code published to Sonarqube for SCA......'
            //    withSonarQubeEnv('sonarqube'){ // You can override the credential to be used
             //        sh 'mvn sonar:sonar'
               // }

           // }

    
        //}
stage('print environment variables')
{
steps{
    echo "the artifact is '${ArtifactId}'"
    echo "the version is '${Version}'"
    echo "the group is '${GroupId}'"
    echo "the name is '${Name}'"
}
}

stage('deploy')
{
steps
{
echo ("deployiong")

}
}
}
    

    }
}