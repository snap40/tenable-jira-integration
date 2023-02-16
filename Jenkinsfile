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
                }
            }
        }

        stage('Get Tenable Vulnerabilities') {
            steps {
                script {
                    try {

                        sh """  
                            export PATH="$HOME/.local/bin:$PATH"
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
