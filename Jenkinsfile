pipeline{
    agent any
    stages
    {
        stage("Clone Code")
        {
            steps
            {
                echo "Cloning the code"
                git url: "https://github.com/SanskarJoshi/django-notes-app.git", branch: "main" 
            }
        }
        stage("Build")
        {
            steps
            {
               echo "Building the image"
              sh "docker build -t note-app ."
            }
        }
        stage("Push to Docker Hub")
        {
            steps
            {
                echo "Pushing the code to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag note-app ${env.dockerHubUser}/my-note-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/my-note-app:latest"
                }
            }
        }
          stage("Deploy")
        {
            steps
            {
                echo "Deploying the code"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
