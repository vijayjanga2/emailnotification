pipeline { 
agent any 
try{
    stages { 
        stage ('Checkout') { 
            steps{
			git 'https://github.com/vijayjanga9/maven-war.git'
				}
         }
		stage ('Compile') { 
            steps{
        sh "mvn clean"
            }
         }
		stage ('Build') { 
            steps{
        sh "mvn package"
            }
         } 
        stage ('Test') { 
            steps{
        sh "mvn test"        
            }
        }
		stage ('Function Test') { 
            steps{
                echo 'functional testing'
            }
         }
		stage ('Security Scan') { 
            steps{
                echo 'Scanning'
            }
         } 
        stage ('QA') { 
            steps{
                echo 'Quality analysis'
            }
        }
        stage ('Deploy') { 
            steps{
                echo 'Deploying'
            }
        }
        stage ('Monitor') {
            steps{
                echo 'Monitoring'
            }
        }
    }           
 currentBuild.result = 'SUCCESS'
}// end of the block
catch (Exception err) {
    echo 'This will run only if failed'
if (currentBuild.rawBuild.getActions(jenkins.model.InterruptedBuildAction.class).isEmpty()) {
    currentBuild.result = 'FAILURE'
    println err
}
else {
        currentBuild.result = 'ABORTED'
}
}
finally {
    echo "Email enbled is : ${params.SEND_EMAIL}"
    color = (currentBuild.result == "SUCCESS") ? "MediumSeaGreen" : "Tomato"
    isLogAttachEnbled = (currentBuild.result == "SUCCESS") ? false : true
    fullDisplayName = "${currentBuild.fullDisplayName}"
    result = "${currentBuild.result}"
    buildUrl = "${env.BUILD_URL}"
    buildNumber = "${currentBuild.number}"
    if ( "${params.SEND_EMAIL}" == "true" ) {
    htmlBodyHead = '''<!DOCTYPE html><html><head><meta name="viewport" content="width=device-width, initial-scale=1"></head>'''
    htmlBody = '''<body><div class="card" style="width: 100%;border: 2px solid ''' + color +''';"><div class="status" style="background-color: ''' + color + ''';text-align: center;color: white;"><h1<''' + result + '''</h1></div><div class="container" style="padding: 2px 16px;"><h4>&nbsp;&nbsp;&nbsp;&nbsp;&nbspPipeline Details: ''' +fullDisplayName + '''</h4><h4>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Build Number: ''' + buildNumber + '''</h4><h4>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp<a href="''' + buildUrl + '''"style="background-color: white;color: black;border: 2px solid ''' + color + ''';padding: 10px 20px;text-align: center;text-docoration: none;display: inline-block;">Click Here to See The Full Log</a></h4></div><br/></div></body></html>'''
    emailext attachLog: isLogAttachEnbled,
    body: htmlBodyHead + htmlBody,
    subject: "Build Status : ${currentBuild.result} || Pipeline Details: ${currentBuild.fullDisplayName}",
    mimeType: 'text/html',
    to: "${params.Email_Recepients}"
    } else {
    echo "No mail will be sent"
    }
} // end of finally block
}
