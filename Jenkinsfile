String branchName = env.BRANCH_NAME
String gitCredentials = "GitHubPersonalAccessToken"
String repoUrl = "https://github.com/dimuit86/jenkins-pipeline.git"

pipeline { 
// 
    environment { 
        registry = "dimuit86/node-demo-app" 
        registryCredential = 'DockerHubAuth' 
        dockerImage = '' 
    }
    agent any 
    stages { 
        stage('Cloning our Git') { 
            steps { 
                git branch: main , credentialsId: 	gitCredentials, url: repoUrl        
            }
        } 
        stage('Building our image') { 
            steps { 
                script { 
                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 
                }
            } 
        }
        stage('Deploy our image') { 
            steps { 
                script { 
                    docker.withRegistry( '', registryCredential ) { 
                        dockerImage.push() 
                    }
                } 
            }
        } 
        stage('Cleaning up') { 
            steps { 
                sh "docker rmi $registry:$BUILD_NUMBER" 
            }
        } 
    }
}