pipeline {
    agent any
    stages { 
//         stage('Checkout') {
//             steps {
//                 git 'https://github.com/akmaharshi/petclinic.git'
//             }
//         }
        stage('Build') {
            tools {
                maven 'M3.9'
                jdk 'java8'
            }
            steps {
                sh "mvn clean package"
            }
        }

//         stage('Sonar') {
//             tools {
//                 maven 'M3.9'
//                 jdk 'java17'
//             }
//             steps {
//                 withSonarQubeEnv('Sonar') {
//                     sh "mvn sonar:sonar -Dsonar.projectKey=petclinic"
//                 }
//             }
//         }
        stage("Post Build Actions") {
            parallel {
//                 stage("Quality Gate") {
//                     steps {
//                         timeout(time: 1, unit: 'HOURS') {
//                             waitForQualityGate abortPipeline: true
//                         }
//                     }
//                 }

                stage('Archive Artifacts') {
                    steps {
                        archiveArtifacts artifacts: 'target/*.?ar', followSymlinks: false
                    }
                }

                stage('Junit Test Report') {
                    steps {
                        junit 'target/surefire-reports/*.xml'
                    }
                }
            }
        }

    }
    post {
        success {
            notify('Success','good')
        }
        failure {
            notify("Failed",'#FF0000')
        }
    }
}

def notify(status,color) {
    emailext (
        to: 'devops.kphb@gmail.com',
        subject: "${status}: Job ${JOB_NAME} [${BUILD_NUMBER}]",
        body: """
            <p>${status}: Job ${JOB_NAME} [${BUILD_NUMBER}] </p>
            <p>Check console output at <a href='${BUILD_URL}'>${JOB_NAME} [${BUILD_NUMBER}]</a></p>
        """
    )
    slackSend color: "${color}", message: "Job ${JOB_NAME} - ${status}, \n Check console output at - ${BUILD_URL}"
}
