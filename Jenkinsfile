pipeline{
    agent any
        environment{
            NEW_VERSION = '1.0'
            SERVER_CREDENTIALS = credentials('testeCred')
        }
    parameters{
        choice(name: 'VERSION',choices: ['1.0','1.1','2.0'],description: 'Desired version')
        booleanParam(name:'executeTests',defaultValue: true, description: "It's self explainable")
    }
        stages{
            stage('test'){
                when{
                    expression{
                        BRANCH_NAME == 'test'
                    }
                }
                steps{
                    echo 'We are in Test Section'
                    sh 'ls -lah'
                }
            }
            stage('prod'){
                when{
                    expression{
                        BRANCH_NAME == 'main'
                    }
                    expression{
                        params.executeTests
                    }
                }
                steps{
                    echo 'We are in main Section'
                    echo "We are building ${NEW_VERSION}"                    
                    withCredentials([
                        usernamePassword(credentialsId: 'testeCred',usernameVariable: 'USER', passwordVariable: 'PWD')
                    ]){
                        sh "echo I am using user ${USER} credential and ${PWD}"
                    }
                    sh 'ls -lah'
                } 
            }
        }
    
    post{
        always{
            echo "Pipeline Always Win"
        }
        success{
            echo "Bingo, we got it"
        }
        failure{
            echo "Ow suck...something was wrong"
        }
    }
}
