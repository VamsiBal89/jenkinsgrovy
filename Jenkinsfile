def gv
pipeline {

    agent any
    parameters{
        string (name: 'NameofJob', defaultValue: 'jenkinssyntaxjb', description: 'version deployed on prod')
        choice (name: 'VERSION', choices: ['1.22', '1.66', '1.44'], description: 'choosing the version out of 3')
        booleanParam (name: 'jobfinished', defaultValue: true, description: 'job is finished or not')
    }
    // By default jenkins support maven, jdk, ant we need to configure them in global tools configuration in Jenkins UI.
    // Now this Maven can be used anywhere in the pipeline.
    tools {
        maven 'Maven'    
    }
    environment {
        NEW_VERSION = '2.1'
        // server_credentials = credentials('server-credentials')
    }
    stages {
        // This is a sample where condition if the condition is true, then all the stages will be executed.
        // when {
        //     expression {
        //         params.jobfinished == true
        //     }
        stage("pre-checks") {
            steps {
                // Here we load groovy script into pipeline by using script block & we define gv as variable at top
                script {
                    gv = load "script.groovy"
                }
            }
        }
        stage("build") {
             when {
                expression {
                    return params.jobfinished == true
                }
            } 
            steps {
                  echo "building the application normally"
                  sh "mvn -v"
                  echo "building application from groovy script"
                  gv.preChecks()
            }
          
        }
        stage("test") {
            steps {
                    echo "testing the application!"
                    echo "The version tested is ${NEW_VERSION}"
            }
        }
        stage('deploying') {
            steps {
            echo "deploying the application!"
            // echo "credentials to deploy is ${server_credentials}"
            // sh "${server_credentials}"
            // The below function fetches the server-credentials defined in Jenkins credentials page & we can use USR and PWD to access them.
            withCredentials([usernamePassword(credentialsId: 'server-credentials', usernameVariable: 'USR', passwordVariable: 'PWD')
            ]) {
                echo "passing credentials to server ${USR} ${PWD}"
                echo "deployed version is ${params.VERSION}"
                }
            }
        }
    }
}

