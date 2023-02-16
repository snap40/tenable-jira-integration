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
        stage('Install plugin') {
            steps {
                script {
                    environment {
                        TENABLE_API_ACCESS_KEY = sh(script: "aws ssm get-parameter --with-decryption --name '/tenable/tenable-api-access-key' --query 'Parameter.Value' --output text", returnStdout: true).trim()
                        TENABLE_API_SECRET_KEY = sh(script: "aws ssm get-parameter --with-decryption --name '/tenable/tenable-api-secret-key' --query 'Parameter.Value' --output text", returnStdout: true).trim()
                        TENABLE_JIRA_API_TOKEN = sh(script: "aws ssm get-parameter --with-decryption --name '/tenable/tenable-jira-api-token' --query 'Parameter.Value' --output text", returnStdout: true).trim()    
                    }
                    
                //    sh "aws sts get-caller-identity" 
                //    def test = sh(script: "aws ssm get-parameter --with-decryption --name '/tenable/tenable-api-access-key' --query 'Parameter.Value' --output text", returnStdout: true).trim()
                //    println test
                           
                    println env.TENABLE_API_ACCESS_KEY
                    println env.TENABLE_API_SECRET_KEY
                    println env.TENABLE_JIRA_API_TOKEN
                    
                    // sh "pip3 install ." 

                }
            }
        }

        // stage('Get Tenable Vulnerabilities') {
        //     steps {
        //         script {
        //             try {

        //                 sh "tenable-jira config.yaml"

                       
        //             }
        //             catch(error) {
        //                 throw error
        //             }
        //         }
        //     }
        // }
    }
}
