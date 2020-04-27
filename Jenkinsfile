pipeline {
    agent any
    tools{
        maven 'local maven'
    }
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo '开始存档....'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to staging'){
            steps{
                echo 'deploy to staging begin'
                build job:'deploy-to-staging'
            }
        }

        stage ('Deploy to Production'){
            steps{
                timeout(time:5, unit:'DAYS'){
                    input message:'是否部署到生產環境?' 
                }

                build job: 'deploy-to-production'
            }
            post {
                success {
                    echo '代碼成功部署到生產環境'
                }

                failure {
                    echo ' 部署失敗'
                }
            }
        }

    }
}