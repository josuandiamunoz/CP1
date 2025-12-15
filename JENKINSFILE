pipeline {
    agent any

     stages {
        stage('Get Code') {
            steps {
                // Obtener código del repo
                git 'https://github.com/josuandiamunoz/CP1.git.git'
                bat 'dir'
                echo WORKSPACE
            }
        }
    
        stage('Build') {
           steps {
                echo 'Esto es python. No hay que compilar.'
                echo WORKSPACE
                bat 'dir'
           }
        }
        
        stage('Tests')
        {
            parallel
            {

                stage('Unit') {
					steps {
						catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
							bat '''
								set PYTHONPATH=%WORKSPACE%
								pytest --junitxml=result-unit.xml test\\unit
							'''
						}
					}
                }
                
                        
                stage('Rest') {
                    steps {
                        catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
                            bat '''
								set FLASK_APP=app\\api.py
								start flask run
								start java -jar C:\\Users\\josu\\Desktop\\unir\\wiremock-standalone-3.13.2.jar --port 9090 --verbose --root-dir test\\wiremock
								set PYTHONPATH=%WORKSPACE%
								pytest --junitxml=result-rest.xml test\\rest
                            '''
                        }
                    }
                }        
            }
        }
        
        stage ('Results') {
            steps {
                junit 'result*.xml'
            }
        }        
     }
}
