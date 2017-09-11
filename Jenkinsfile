TASK='1'

node (){
	def JAVA_HOME = tool 'JDK8'

	properties([pipelineTriggers([pollSCM('* * * * *')])])
    
    stage ("Donwload Sources") {
		try {
			git url: 'https://github.com/pablotoledo/scala-maven.git'
		} catch (err) {
			sh "exit -1"
		}
	}

	stage ("Compile Source") {
		try{
			withEnv(["JAVA_HOME=${JAVA_HOME}"]) {
				sh "echo ${JAVA_HOME}"
                sh "env"
                sh "ls -la"
				sh "mvn clean compile package"
			}
		} catch (err) {
			sh "exit -1"
		}
	}

    	stage ("UnitTest Source") {
		try{
			withEnv(["JAVA_HOME=${JAVA_HOME}"]) {
				sh "echo ${JAVA_HOME}"
				sh "mvn clean test"
			}
		} catch (err) {
			sh "exit -1"
		}
	}

    stage ("CoverageTest Source") {
		try{
			withEnv(["JAVA_HOME=${JAVA_HOME}"]) {
				sh "echo ${JAVA_HOME}"
				sh "mvn scoverage:report sonar:sonar"
			}
		} catch (err) {
			sh "exit -1"
		}
	}

	stage ("Publish Artifact Nexus") {
		try{
			withEnv(["JAVA_HOME=${JAVA_HOME}"]) {
				sh "echo ${JAVA_HOME}"
				sh "mvn deploy"
			}
		} catch (err) {
			sh "exit -1"
		}
	}

	

}
