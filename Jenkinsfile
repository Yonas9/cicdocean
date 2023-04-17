pipeline {

  agent any
  environment {
    //adding a comment for the commit test
    DEPLOY_CREDS = credentials('deploy-anypoint-user')
    MULE_VERSION = '4.4.0'
    BG = "SelfTaught"
    WORKER = "Micro"
    M2SETTINGS ="C:\\Users\\Yonas_Eluna\\.m2\\settings.xml"
  }
  stages {
    stage('Build') {
      steps {
            bat 'mvn -B -U -e -V clean -gs %M2SETTINGS% -DskipTests package'
      }
    }

    stage('Test') {
      steps {
          bat "mvn test"
      }
    }

     stage('Deploy Development') {
      environment {
        ENVIRONMENT = 'Sandbox'
        APP_NAME = 'blueocean'
      }
      steps {
       script {
                    def yamlDoc = """
                    environment:
                      cloudhub:
                        - Sandbox
                        - Production
                    """
                    def yamlMap = readYaml(text: yamlDoc)
                    
                    // Use the `yamlMap` variable to access the YAML document in your pipeline
                    echo "The cloudhub environment contains: ${yamlMap.environment.cloudhub}"
                }
            bat 'mvn -U -V -e -B -gs %M2SETTINGS% -DskipTests deploy -DmuleDeploy -Dmule.version="%MULE_VERSION%" -Danypoint.username="%DEPLOY_CREDS_USR%" -Danypoint.password="%DEPLOY_CREDS_PSW%" -Dcloudhub.app="%APP_NAME%" -Dcloudhub.environment[0]="%ENVIRONMENT%" -Dcloudhub.bg="%BG%" -Dcloudhub.worker="%WORKER%"'
      }
    }
    stage('Deploy Production') {
      environment {
        ENVIRONMENT = 'Production'
        APP_NAME = 'blueocean'
      }
      steps {
       script {
                    def yamlDoc = """
                    environment:
                      cloudhub:
                        - Sandbox
                        - Production
                    """
                    def yamlMap = readYaml(text: yamlDoc)
                    
                    // Use the `yamlMap` variable to access the YAML document in your pipeline
                    echo "The cloudhub environment contains: ${yamlMap.environment.cloudhub}"
                }
            bat 'mvn -U -V -e -B -gs %M2SETTINGS% -DskipTests deploy -DmuleDeploy -Dmule.version="%MULE_VERSION%" -Danypoint.username="%DEPLOY_CREDS_USR%" -Danypoint.password="%DEPLOY_CREDS_PSW%" -Dcloudhub.app="%APP_NAME%" -Dcloudhub.environment[1]="%ENVIRONMENT%" -Dcloudhub.bg="%BG%" -Dcloudhub.worker="%WORKER%"'
      }
    }
  }
}