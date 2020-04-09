pipeline {
    agent any
  
    parameters { 
        string(name: 'image_tag' , defaultValue: "", description: 'Enter image_tag')
        
    }

    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image') {
          
            steps {
                buildName "${params.image_tag}"
                script {
                sh "sudo docker build -t saiakhil46/ownkubeproject:${params.image_tag} ."
                }   
            }
        }
        stage('Push Docker Image') {
          
            steps {
                
                withCredentials([usernamePassword(credentialsId: 'docker_hub_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                sh "sudo docker login -u $USERNAME -p $USERPASS"
                sh "sudo docker push saiakhil46/ownkubeproject:${params.image_tag}"

                }
            }
        }
    }
}
