pipeline {
    agent any
    
    
    stages {
        stage('Cleanup') {
            steps {
                script {
                    
                    cleanWs()
                }
            }
        }
        
        stage('install astro cli and clone repo'){
            steps{
               sh '''
               curl -sSL install.astronomer.io | sudo bash -s -- v1.30.0
               astro version
               
               git clone https://github.com/Nithin-Kumar-01/dbt-airflow-poc.git
               cd dbt-airflow-poc 
               astro dev start
                '''
            }
        }
        
        
    }
}
