pipeline{
    agent any

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
    }
   
}