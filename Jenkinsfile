TASK='1'

node (){
	def MAVEN_HOME = tool 'MAVEN'
	def JAVA_HOME = tool 'JDK8'

	properties([pipelineTriggers([pollSCM('* * * * *')])])

	stage ("Compile Source") {
		try{
			withEnv(["JAVA_HOME=${JAVA_HOME}"]) {
				sh "echo ${JAVA_HOME}"
                sh "env"
                sh "ls -la"
				sh "${MAVEN_HOME}/bin/mvn clean compile"
			}
		} catch (err) {
			sh "exit -1"
		}
	}

	

}
