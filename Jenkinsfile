pipeline
{
    agent any
    stages
    {
        stage('Build Backend')
        {
            steps
            {
                bat 'mvn clean package -DskipTests=true'
            }

        }
        stage('Unit Tests')
        {
            steps
            {
                bat 'mvn test'
            }

        }
        stage('Sonar Analysis')
        {

            environment
            {
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps
            {
                withSonarQubeEnv('SONAR_LOCAL')
                {
                    bat "\"${scannerHome}/bin/\"sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=6b0460253fdbb82147101b44a04ccacb43fb9c3f -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**Application.java"
                    
                }
            }

        }
        stage ('Quality Gate')
        {
            steps
            {
              
                sleep(45)
                
                timeout(time: 1, unit: 'MINUTES')
                {
                    
                    waitForQualityGate abortPipeline: true
                                
                }
            
               
            }
        }
        stage ('Deploy Backend')
        {
            steps
            {

                 deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'


            }

        }
    }


}