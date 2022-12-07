node{
    
    def mavenHome = tool name: "maven3.8.6"
    timestamps {
        echo "The Build Number is: ${env.BUILD_NUMBER} "
        echo "The jenkins URL is: ${env.JENKINS_URL} "
        echo "The workspace is: ${env.WORKSPACE} "
        echo "  The Tag date is: ${env.TAG_DATE} "
     properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([cron('''* * * * *
'''), pollSCM('* * * * *')])])
        

    stage('Checkout Code'){
        git branch: 'development', credentialsId: 'github', url: 'https://github.com/naveendevopsorgi/maven-web-application.git'
    }

    stage('Build'){
        sh "$mavenHome/bin/mvn clean package"
    }
    
    stage('sonarQubecode'){
        sh "$mavenHome/bin/mvn sonar:sonar"
    }
    
    stage('ArtifactoryRepo'){
        sh "$mavenHome/bin/mvn deploy"
    }
    
    stage('DeploytoTomcat'){
        sshagent(['2612922d-d77a-4c8b-b475-57fff49293ae']) {
            sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.228.21.152:/opt/apache-tomcat-9.0.69/webapps"
}
    }
    }
}//Node Closing
