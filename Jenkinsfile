pipeline{
    agent any

    environment {
        COMMIT_HASH = sh(returnStdout: true, script: 'git rev-parse --short=4 HEAD').trim()
        BRANCH = "${env.GIT_BRANCH}"
        TAG = "${env.BRANCH}.${env.COMMIT_HASH}.${env.BUILD_NUMBER}".drop(15)
        DEV_TAG = "${env.BRANCH}.${env.COMMIT_HASH}.${env.BUILD_NUMBER}".drop(7)
        VERSION = "${env.TAG}"
        max = 50
        random_num = "${Math.abs(new Random().nextInt(max+1))}"
        docker_username = "${env.username}"
        docker_password = "${env.password}"
    }

    stages{
        stage("Workspace Cleanup") {
            steps {
                dir("${WORKSPACE}") {
                    deleteDir()
                }
            }
        }

        stage('Checkout Git') {
            steps {
                git branch: 'main', credentialsId: '1f538342-affb-4763-b4dd-70aa119ae380', url: 'https://github.com/Micah-Shallom/php-todo-proj20.git'
            }
        }
        stage('Building application ') {
            steps {
                script {
                    sh " docker login -u {docker_username} -p {docker_password}"
                    sh " docker build -t mshallom/todo-proj20:${env.version} ."
                }
            }
        }
        stage('Creating docker container') {
            steps {
                script {
                    sh " docker run -d --name todo-app-${env.random_num} -p 8000:8000 mshallom/todo-proj20:${env.version}"
                }
            }
        }
        stage("Smoke Test") {
            steps {
                script {
                    sh "sleep 5"
                    sh "curl -I 54.226.249.187:8000"
                }
            }
        }
        stage("Publish to Registry") {
            steps {
                script {
                    sh " docker push mshallom/todo-proj20:${env.version}"
                    // sh " docker login -u {docker_username} -p {docker_password}"
                }
            }
        }
        stage ('Clean Up') {
            steps {
                script {
                    sh " docker stop todo-app-${env.random_num}"
                    sh " docker rm todo-app-${env.random_num}"
                    sh " docker rmi mshallom/todo-proj20:${env.version}"
                }
            }
        }

        stage ('logout Docker') {
            steps {
                script {
                    sh " docker logout"
                }
            }
        }
    }
   
}