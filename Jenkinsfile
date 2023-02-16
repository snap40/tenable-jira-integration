//pip3 install .  - in root dir of repo
//tenable-jira config.yaml            
//cron every day or week??
//aws ssm get-parameter --with-decryption --name 'YOURKEY' --query 'Parameter.Value' --output text

pipeline {
    options {
        ansiColor('xterm')
    }
    
    triggers {
        // Run 8AM Monday - Friday
        cron('0 8 * * 1-5')
    }

    agent {
        label 'linux && docker' 
    }

    stages {
        stage('Initialise Parameters') {
            steps {
                script {
                   env.TENABLE_API_ACCESS_KEY = sh(script: "aws ssm get-parameter --with-decryption --name '/tenable/tenable-api-access-key' --query 'Parameter.Value' --output text", returnStdout: true).trim()
                   env.TENABLE_API_SECRET_KEY = sh(script: "aws ssm get-parameter --with-decryption --name '/tenable/tenable-api-secret-key' --query 'Parameter.Value' --output text", returnStdout: true).trim()
                   env.TENABLE_JIRA_API_TOKEN = sh(script: "aws ssm get-parameter --with-decryption --name '/tenable/tenable-jira-api-token' --query 'Parameter.Value' --output text", returnStdout: true).trim()    

                    sh "sed -i 's/PLACEHOLDER-TENABLE-API-ACCESS-KEY/${env.TENABLE_API_ACCESS_KEY}/g' config.yaml"
                    sh "sed -i 's/PLACEHOLDER-TENABLE-API-SECRET-KEY/${env.TENABLE_API_SECRET_KEY}/g' config.yaml"
                    sh "sed -i 's/PLACEHOLDER-TENABLE-JIRA-API-TOKEN/${env.TENABLE_JIRA_API_TOKEN}/g' config.yaml"

                            
                    println env.TENABLE_API_ACCESS_KEY
                    println env.TENABLE_API_SECRET_KEY
                    println env.TENABLE_JIRA_API_TOKEN
                }
            }
        }

        stage('Get Tenable Vulnerabilities') {
            steps {
                script {
                    try {

                        sh """  
                            sudo pip3 install .
                            sudo tenable-jira config.yaml
                        """
                       
                    }
                    catch(error) {
                        throw error
                    }
                }
            }
        }
    }
}
