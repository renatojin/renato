pipeline {
      agent any
      parameters {
        string(
            name: 'ARTIFACT_NAME',
            defaultValue: 'build_pipeline',
            description: 'mjenja se ime fajla'
            )
    }

      stages {
            stage('Download') {
                  steps {
                      cleanWs()
                      echo (message: "Download…")
                  dir('pipeline'){
                      git (branch: 'pipeline',
                      url: 'https://github.com/rtrk-jenkins-course/jenkins-course.git')
                  }
                  rtDownload (
                      serverId:'local-artifactory',
                      spec: '''{
                          "files": [
                          {
                          "pattern": "example-repo-local/printer.zip",
                          "target": "pipeline/"
                          }
                          ]
                      }'''
                      )
                    
                    unzip (
                        zipFile: "pipeline/printer.zip",
                        dir: "pipeline/"
                        )
                  }
            }
            stage('Build') {
                  steps {
                        echo (message: "Build…")
                        dir("pipeline"){
                            
                        
                        bat (
                            script: """
                            call Makefile.cmd
                            """
                            )
                        }
                  }
            }
         stage("Publish") {
                steps{
                      script{
                        echo "message: Publish…"
                        bat 'exit 1'
                        zip (
                            zipFile: "${ARTIFACT_NAME}.zip",
                            archive: true,
                            dir: "pipeline/"
                            )
                      }
                     // dir("Pipa-projekt"){
                      rtUpload (
                      serverId:'local-artifactory',
                      spec: '''{
                          "files": [
                          {
                          "pattern": "${ARTIFACT_NAME}.zip",
                          "target": "example-repo-local/release/"
                          }
                          ]
                      }'''
                      )
               // }
                }
         }
      }
      
      post { 
        always { 
            mail (
                to: "renato.kragulj@rt-rk.com",
                subject: "test maila",
                body: "${env.JOB_NAME} ${env.BUILD_ID}"
                )
        }
    }

}





