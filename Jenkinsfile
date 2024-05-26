pipeline {
    agent any

    stages {
        stage('Build Artifact') {
            steps {
                sh "mvn clean package -DskipTests=true"
                archive 'target/*.jar' // so that they can be downloaded later
            }
        }
//         stage('Test Artifact') {  // Example of a different stage name
//             steps {
//                 sh "mvn test"
//                 junit 'target/surefire-reports/*.xml' // example of a test report
//             }
//         }
        stage('Unit Tests - JUnit and Jacoco') {
              steps {
                sh "mvn test"
              }
              post {
                always {
                  junit 'target/surefire-reports/*.xml'
                  jacoco execPattern: 'target/jacoco.exec'
                }
              }
        }

        stage('Docker Build and Push') {
              steps {
                withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
                  sh 'printenv'
                  sh 'docker build -t yasiru1997/numeric-app4:""$GIT_COMMIT"" .'
                  sh 'docker push yasiru1997/numeric-app4:""$GIT_COMMIT""'
                  sh 'docker ps'
                }
              }
        };//



    }
}
