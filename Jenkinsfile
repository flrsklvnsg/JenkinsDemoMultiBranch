def gv

pipeline {
    agent any
    //{label "linux"}
    options{
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr:'5')
        disableConcurrentBuilds()
    }
    parameters{
        string(name: 'VERSION', defaultValue: '', description: 'version to deploy on prod')
        choice(name: 'VERSION', choices: ['1.1.0','1.2.0','1.3.0'], description: '')
        booleanParam(name: 'executeTests', defaultValue: true, description: '')
    }
    stages {
         stage('init') {
            steps {
                script{
                    gv = load "script.groovy"

                }
            }
        }
        stage('Build') {
            steps {
                script{
                    gv.buildApp()
                } 

            }
        }
            stage('Test') {
            steps {
           /*     when{
                    expression{
                        params.executeTests
                    }
               }
                */
                 script{
                     if(params.executeTests == true){
                        gv.testApp()
                     }
                   
                } 
            }
        }
        stage('Deploy') {
            steps {
                script{
                    gv.deployApp()
                } 
                }
        }
          stage('cat README') {
            when{
                branch "fix-*"
            }
            steps {
                sh '''
                cat README.md
                '''
                }
        }
        
    }
}
    
