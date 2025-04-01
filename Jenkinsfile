pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Clone the Git repository containing JMeter test scripts
                git branch: 'main', url: 'https://github.com/6304359173/jmeter-scripts.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                // You can add build steps if you have a project that needs building
            }
        }

        stage('Test') {
            steps {
                echo 'Running JMeter tests...'

                // Assuming JMeter is installed and accessible in the Jenkins environment
                // Run JMeter tests in non-GUI mode (-n) with the .jmx script and output the results to a .jtl file
                sh '''
                    jmeter -n -t jmeter-scripts/JSON2.jmx -l jmeter-scripts/results/result.jtl
                '''
            }
        }

        stage('Publish Results') {
            steps {
                echo 'Publishing JMeter results...'
                
                // Optional: Use Jenkins Performance Plugin to publish and visualize results
                // Adjust the path to match where your .jtl file is saved
                perfReport(
                    sourceData: 'jmeter-scripts/results/result.jtl',
                    thresholds: [
                        absolute: [warning: 1000, critical: 1500] // Customize these thresholds based on your needs
                    ]
                )
            }
        }
    }

    post {
        always {
            // Cleanup or notifications can go here, for example, sending an email if the test fails
            echo 'Cleaning up after the test...'
        }

        success {
            echo 'Test succeeded!'
        }

        failure {
            echo 'Test failed!'
            // Optionally send an alert if the test fails
            . Mail it to: 'sigharam01@gmail.com',
                 subject: "JMeter Test Failed",
                 body: "The performance test failed. Please check Jenkins for the results."
        }
    }
}
