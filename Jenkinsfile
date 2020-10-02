properties([[$class: 'JiraProjectProperty'], buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '5', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
pipeline {
    agent any
    tools {
    maven 'Maven_Home.3.6.3'
    }
    stages {
        stage("SCM_checkout") {
            steps {
              git branch: 'stage', credentialsId: 'github_id', url: 'https://github.com/githubjigalooru/spring-boot-mongo-docker.git'  
            }
        }
        stage("build_artifact") {
            steps {
                sh label: '', script: 'mvn clean install'
            }
        }
        stage("build_image") {
            steps {
                sh label: '', script: 'docker build -t amruth0005/spring_mongo:${BUILD_NUMBER} .'
            }
        }
        stage("Push_image_to_dockerhub") {
            steps {
                withCredentials([string(credentialsId: 'amr_hub', variable: 'Docker_hub')]) {
                sh label: '', script: 'docker login -u amruth0005 -p ${Docker_hub}'
                }
                sh label: '', script: 'docker push amruth0005/spring_mongo:${BUILD_NUMBER}'
            }
        }
    }
}
