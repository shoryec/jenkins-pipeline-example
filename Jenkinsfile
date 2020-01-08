#!/usr/bin/env groovy
pipeline {
    agent {docker { image 'python:3.5.1' }}
    parameters {
        choice( name: 'environmentType', choices: ['Non-Production', 'Production'], description: 'Select the relevant Environment type')
    }
    stages {
        stage( 'Set Environment' ){
            steps {
                script {

                    def currentEnvironment
                    if ( params.environmentType == "Non-Production" ) {
                        currentEnvironment = "Non-Production"
                    } else if ( params.environmentType == "Production" ) {
                        currentEnvironment = "Production"
                    }
                    env['ENV_NAME'] = currentEnvironment
                    echo "Execution will be against ${env.ENV_NAME}"
                }
            }
        }
        // Stage to retrieve the User of the current build requester
        // Stage can be used along with RBAC plugins to check the groups the user
        // has been added to and proceed accordingly
        // Here we ask the user for their username, but with the right plugins installed
        // this process can be automated
        stage('Retireve User'){
            
            steps{
                script{  
                    def build_user = input message: 'Request to Proceed?', submitterParameter: 'user'
                    echo "Build User is ${build_user}"
                    // Can add if command here to check if the user has the right
                    // accesspermissions or restrict running the next stage
                    // to only a select set of users/usernames
                }
            }
            
            
        }
        stage('Build') {
            steps {
                sh "python --version"
                // Can run a python command here to compile specific .py files. Example below:-
                // sh 'python -m py_compile sources/hello.py sources/world.py'
            }
        }

        stage("Test") {
            input {
                message "Should tests begin?"
            }
            steps{
                // Example command : sh py.test --verbose --junit-xml test-reports/results.xml sources/*.py'
                echo "Test Complete"
            }
        }

        stage( 'Push to Image Production Registry' ){
            steps{
                script{
                    def confirmation = input message: 'Push to image registry?'
                    echo "Pushed to image registry"
                }
            }
        }
    }
}