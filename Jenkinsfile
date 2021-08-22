node('master') {

    stage('Preparation') {
        sh '''export MAVEN_HOME=/opt/maven
            export PATH=$PATH:$MAVEN_HOME/bin
            mvn --version
            mvn clean package'''
    }

    stage('Poll') { 
        checkout scm
    } 
    
    stage('Build & Unit test') {
        sh 'mvn clean verify -DskipITs=true'; 
        junit '**/target/surefire-reports/TEST-*.xml' 
        archive 'target/*.jar'
    }
    
    stage('Static Code Analysis') { 
        sh 'mvn clean verify sonar:sonar -Dsonar.projectName=jenkins-sonarqube-demo -Dsonar.projectKey=jenkins-sonarqube-demo -Dsonar.projectVersion=$BUILD_NUMBER';
    } 
    
    stage ('Integration Test') {
        sh 'mvn clean verify -Dsurefire.skip=true'; 
        junit '**/target/failsafe-reports/TEST-*.xml' 
        archive 'target/*.jar'
    }
    
}
