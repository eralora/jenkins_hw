#!/usr/bin/env groovy

@Library('jenkins-shared-library')

def gv 

pipeline {
    agent any
    tools {
        nodejs "node"
    }
    stages {
        stage("Increment version"){
            steps {
                script {
                        IncrementVersion()
                    } 
                }
            }
        }
        stage("Run tests"){
            steps {
                script {
                    RunTests()
                }
                    
            }
        }
        stage("Build and push Docker image"){
            steps {
                script {
                    BuildAndPushDockerImage()
                    }
                }
            }
        stage("Commit version update"){
            steps {
                script {
                    CommitVersionUpdate()
                }
            }
        }
    }
        
}