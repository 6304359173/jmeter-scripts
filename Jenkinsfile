pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Clones the repository containing JMeter test scripts
                git branch: 'main', url: 'https://github.com/6304359173/jmeter-scripts.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
            }
        }

        stage('Test') {
            steps {
                echo 'Running JMeter tests...'                
                bat 'C:\\apache-jmeter-5.6.3\\bin\\jmeter -n -t C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\Jmete_scm\\jmeter-scripts\\JSON2.jmx -l C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\Jmete_scm\\jmeter-scripts\\results\\result.jtl'


            }
        }

        stage('Publish Results') {
            steps {
                echo 'Publishing JMeter results...'
                
                // Corrected perfReport syntax
                perfReport(
                    sourceDataFiles: 'jmeter-scripts/results/result.jtl',
                    parsers: [[$class: 'JMeterParser']]
                )
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed!'
        }

        success {
            echo 'JMeter test succeeded!'
        }

        failure {
            echo 'JMeter test failed!'
            
            // Corrected email notification syntax
            emailext(
                to: 'sigharam01@gmail.com',
                subject: "JMeter Test Failed",
                body: "The performance test failed. Please check Jenkins for details."
            )
        }
    }
}
