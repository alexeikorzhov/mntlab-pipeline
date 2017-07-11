node (env.SLAVE) {
	stage('Preparation') {
		git url:'https://github.com/MNT-Lab/mntlab-pipeline.git', branch: 'asemirski'
	}
    stage('Gradle Build') {
        sh "/opt/gradle/bin/gradle build"
    }
    stage ('Testing') {
    	parallel (
    		cucumber: {
    			stage ('cucumber') {
    				sh "/opt/gradle/bin/gradle cucumber"
    			}
    		},
    		jacoco: {
    			stage ('jacoco') {
    				sh "/opt/gradle/bin/gradle jacocoTestReport"
    			}
    		},
    		unit: {
    			stage ('unit test') {
    				sh "/opt/gradle/bin/gradle test"
    			}
    		}
    	)
    }
    stage ('Triggering job and fetching artefact after finishing') {
    	build job: "MNTLAB-asemirski-child1-build-job", parameters: [string(name: 'BRANCH_NAME', value: "asemirski")]
    }
}
    