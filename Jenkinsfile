pipeline {
    agent any

    environment {
        BD_PROJECT_NAME = 'Samplepipeline' // Replace with your Black Duck project name
        BD_VERSION_NAME = '1.0' // Replace with your version name
        BD_SCAN_NAME = 'JenkinsScan' // Replace with a name for your scan
        BD_API_TOKEN = 'NjI3YjYyY2ItZTIzOS00NTEwLWEyNjYtMTRjZjg2MTZlZTFkOjVhODc2ODMwLTRmYTctNDRmYS1iMGQ3LTI0NzI1MzVjYTY2Nw==' // Replace with your actual token
        BD_URL = 'https://poc416.blackduck.synopsys.com/' // Replace with your Black Duck server URL
        SOLUTION_NAME = 'AspnetCoreMvcFull.sln' // Replace with your .NET solution file name
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Hclishikawa/materio-bootstrap-html-aspnet-core-mvc-admin-template-free.git'
            }
        }

        stage('Restore NuGet Packages') {
            steps {
                sh 'dotnet restore ${SOLUTION_NAME}'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build ${SOLUTION_NAME} -c Release'
            }
        }

        stage('Black Duck Scan') {
            steps {
                withEnv(['HEADLESS=true']) {
                    sh "/opt/SynopsysDetect/synopsys-detect --blackduck.url=${BD_URL} --blackduck.api.token=${BD_API_TOKEN} --blackduck.trust.cert=true --detect.project.name=${BD_PROJECT_NAME} --detect.project.version.name=${BD_VERSION_NAME} --detect.code.location.name=${BD_SCAN_NAME} --detect.source.path=${SOLUTION_NAME} --detect.risk.report.pdf=true --detect.risk.report.pdf.path=risk-report.pdf"
                }
            }
        }
    }
}
