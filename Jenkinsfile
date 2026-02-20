@Library('Shared')_
pipeline{
    agent{
        node{
            label 'dev'
        }
    }
    
    stages{
        stage('clone code'){
            steps{
                git url: "https://github.com/priyanshu7111/django-notes-app.git", branch: "main"
            }
        }
        stage('build & test'){
            steps{
                sh "docker build -t notes-app:latest ."
            }
        }
        stage('Push image to dockerhub'){
            steps{
                withCredentials(
                    [usernamePassword(
                          credentialsId:"dockerCreds",
                          passwordVariable:"dockerHubPass",
                          usernameVariable:"dockerHubUser"
                        )
                    ]
                ){
                    sh "docker image tag notes-app:latest ${env.dockerHubUser}/notes-app:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/notes-app:latest"
                }
            }
        }
        stage('deploy'){
            steps{
                sh "docker compose up -d"
            }
        }
    }
}
