pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
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

        stage('publish to nexus'){
            steps{
                nexusArtifactUploader artifacts: [[artifactId: 'sushmaDevOpsLab', classifier: 'war', file: 'target/sushmaDevopsLab-0.0.11-SNAPSHOT.war', type: 'war']], credentialsId: '', groupId: 'com.sushmadevopslab', nexusUrl: '172.20.10.143:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'sushmadevopslab_SNAPSHOT', version: '0.0.11'
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
           stage('deploy'){
        steps{
        echo 'deploying;.......'
    }
}
        
        
    }

}