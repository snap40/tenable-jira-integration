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
                   def tenable-api-access-key = sh(script: "aws ssm get-parameter --with-decryption --name '/tenable/tenable-api-access-key' --query 'Parameter.Value' --output text", returnStdout: true).trim()
                   def tenable-api-secret-key = sh(script: "aws ssm get-parameter --with-decryption --name '/tenable/tenable-api-secret-key' --query 'Parameter.Value' --output text", returnStdout: true).trim()
                   def tenable-jira-api-token = sh(script: "aws ssm get-parameter --with-decryption --name '/tenable/tenable-jira-api-token' --query 'Parameter.Value' --output text", returnStdout: true).trim()    

                    println tenable-api-access-key
                    println tenable-api-secret-key
                    println tenable-jira-api-token
                    
                    sh "sed -i 's/PLACEHOLDER-TENABLE-API-ACCESS-KEY/${tenable-api-access-key}/g' config.yaml"
                    sh "sed -i 's/PLACEHOLDER-TENABLE-API-SECRET-KEY/${tenable-api-secret-key}/g' config.yaml"
                    sh "sed -i 's/PLACEHOLDER-TENABLE-JIRA-API-TOKEN/${tenable-jira-api-token}/g' config.yaml"

                            
                   
                }
            }
        }

        stage('Get Tenable Vulnerabilities') {
            steps {
                script {
                    try {

                        sh """  
                        pip3 install .
                        tenable-jira config.yaml
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
