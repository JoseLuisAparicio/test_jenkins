TASK='1'

node (){
	def MAVEN_HOME = tool 'MAVEN'
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
				sh "${MAVEN_HOME}/bin/mvn clean compile package"
			}
		} catch (err) {
			sh "exit -1"
		}
	}

    	stage ("UnitTest Source") {
		try{
			withEnv(["JAVA_HOME=${JAVA_HOME}"]) {
				sh "echo ${JAVA_HOME}"
				sh "${MAVEN_HOME}/bin/mvn clean test"
			}
		} catch (err) {
			sh "exit -1"
		}
	}

    stage ("CoverageTest Source") {
		try{
			withEnv(["JAVA_HOME=${JAVA_HOME}"]) {
				sh "echo ${JAVA_HOME}"
				sh "${MAVEN_HOME}/bin/mvn scoverage:report"
			}
		} catch (err) {
			sh "exit -1"
		}
	}

	stage ("Publish Artifact Nexus") {
		try{
			withEnv(["JAVA_HOME=${JAVA_HOME}"]) {
				sh "echo ${MAVEN}"
				sh "echo ${JAVA_HOME}"
				sh "GROUP_ID=`echo -e 'setns x=http://maven.apache.org/POM/4.0.0\ncat /x:project/x:groupId/text()' | xmllint --shell pom.xml | grep -v /`"
				sh "ARTIFACT_ID=`echo -e 'setns x=http://maven.apache.org/POM/4.0.0\ncat /x:project/x:artifactId/text()' | xmllint --shell pom.xml | grep -v /`"
				sh "VERSION=`echo -e 'setns x=http://maven.apache.org/POM/4.0.0\ncat /x:project/x:version/text()' | xmllint --shell pom.xml | grep -v /`"
				sh "mvn deploy:deploy-file -Durl=http://172.17.0.5:8081/nexus/content/repositories/snapshots -Dfile=target/${ARTIFACT_ID}-${VERSION}.jar -Dpackaging=jar -DgroupId=$GROUP_ID -DartifactId=$ARTIFACT_ID -Dversion=$VERSION -DrepositoryId=snapshots"
			}
		} catch (err) {
			sh "exit -1"
		}
	}

	

}
