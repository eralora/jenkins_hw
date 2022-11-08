pipeline {
    agent any
    tools {
        nodejs "node"
    }
    stages {
        stage("Increment version"){
            steps {
                script {
                    dir("app"){
                        npm version minor 
                        def packageJson = readJson file: 'package.json'
                        def version = packageJson.version
                        env.IMAGE_NAME = "$version-$BUILD_NUMBER"
                    } 
                }
            }
        }
        stage("Run tests"){
            steps {
                script {
                    dir("app"){
                        sh "npm install"
                        sh "npm run test"
                    }
                    }
                    
                }
            }
        stage("Build and push Docker image"){
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'nexus-docker-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh "docker build -t 207.154.233.102:8090/my-node-app:$IMAGE_NAME ."
                        sh "echo $PASS | docker login -u $USER --password-stdin 207.154.233.102:8090"
                        sh "docker push 207.154.233.102:8090/my-node-app:$IMAGE_NAME" 
                    }
                    }
                }
            }
        stage("Commit version update"){
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'github-credentials', passwordVariable: 'PWD', usernameVariable 'USER')]){
                        sh 'git config  --global user.email "jenkins@example.com"'
                        sh 'git config  --global user.name "jenkins"'
            
                        sh "git remote set-url origin https://${USER}:${PWD}@github.com/eralora/jenkins_hw.git"
                        sh 'git add .'
                        sh 'git commit -m "ci: version bump"'
                        sh 'git push origin HEAD:jenkins-jobs'
                    }
                }
            }
        }
    }
        
}